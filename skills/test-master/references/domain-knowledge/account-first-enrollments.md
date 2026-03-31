# Account First Enrollments (AFE)

Source: [Miro Board — Account First Enrollments](https://miro.com/app/board/uXjVKOofDy0=/?moveToWidget=3458764634015792675)

## Overview

Account First Enrollment (AFE) is the onboarding flow for new users creating an account on the MyVU platform (designed for Veterans and Military Families). Unlike login flows where the user already exists, AFE handles the case where a user may or may not already have a Profile and/or Contact record in the system.

Two parallel flow variants exist:
- **MVP Flow** — without PING Protect; uses delayed validation to avoid account enumeration vulnerabilities
- **Preferred Flow** — with PING Protect enabled; allows instant user feedback during email validation

---

## Key Concepts

| Term | Definition |
|---|---|
| **Profile** | Authentication record used by PING identity management |
| **Contact** | CRM record containing borrower data (name, email, phone) |
| **CIS** | Client Identification Service — matches user input to existing records |
| **DaVinci Flow** | PING orchestration flow that handles the enrollment steps |
| **enrollmentGuid** | Unique identifier added to Profile table to track enrollment state |
| **Incomplete Enrollment Group** | PING group used to track users who have not completed enrollment |
| **Magic Link** | Email link that completes email verification without manual code entry |
| **NPS** | Non-Purchasing Spouse — special handling required to avoid revealing this status |
| **IDVQ** | Identity Verification Questions — fallback authentication method |

---

## System Integration Points

| System | Role |
|---|---|
| **MyVU** | Front-end platform (deployed behind feature flag — OTPI-1056) |
| **Profile API** | Creates and retrieves user profiles; extended with enrollmentGuid |
| **Contact API** | Creates CRM contact records post-verification |
| **PING / DaVinci** | Identity orchestration; manages OTP, MFA, device registration |
| **CRM** | Stores contact data; queried to check for existing contacts |
| **CIS (Client Identification Service)** | Matches email/phone input to existing lead/contact records |
| **SendGrid** | Email delivery for OTP codes and magic links |
| **RabbitMQ** | Message bus for ProfileAPI.CrmUpdate consumers |

---

## AFE Sign Up Flow

### Stage 1: Entry

1. User lands on the MyVU account creation screen
2. User enters their **email address**

---

### Stage 2: Validation

3. System calls **Profile API** to check for an existing profile with that email
4. System calls **CRM** to check for an existing Contact record
5. **CIS** attempts to match input to an existing lead or contact

**Branch: Existing User Found**
- User is redirected to the **login flow** instead of continuing enrollment

**Branch: New User**
- Proceeds to email verification

---

### Stage 3: Email Verification

6. System sends an **8-digit OTP code** to the entered email address; account is created in Ping with **"unverified"** tag on the email
7. User enters the OTP code on the verification screen
8. Email is verified ✓ — Ping updates the email status to **"verified"**
   - If email verification is not completed, every subsequent regular login or AFE login will continue sending the 8-digit OTP code until the email is verified

---

### Stage 4: Contact Creation

9. If no CRM Contact exists → system **creates a new Contact** record
10. Contact is linked to the Profile via `contactGuid`
11. CRM is updated with:
    - **Enrollment Link**
    - **Enrollment Date**
    - **Login Date**

> Contact creation is deferred until **after** email verification to avoid orphaned records

---

### Stage 5: Password Setup

12. User is asked to create a password with the option to skip
13. Future: a reminder to set a password may be shown 24 hours after enrollment

> **If the user skips:** No password is set on the account.
> In **Profile Settings**, the password action is labeled **"Create Password"** (not "Change Password") since no password exists yet.


---

### Stage 6: Platform Entry

14. User lands in **MyVU dashboard**
15. Platform displays loan offerings appropriate to the user's profile/contact status


---

## Flow Diagram Summary

```
User enters email
        │
        ▼
Check Profile API + CRM + CIS
        │
   ┌────┴────┐
Exists?     New?
   │          │
Login OTP   Email OTP (8-digit code)
Flow             │
            Email verified ✓
                 │
          Create Contact (if none)
                 │
        Password Setup (required)
                 │
        Enter MyVU Dashboard
```

---

## OPQ Enrollment Path

For users entering through the **OPQ (One-Person Queue)** flow:

- The enrollment link source parameter (`?source=`) routes the user to the appropriate path:
  - `source=email` → Email OTP path
  - `source=phone` → Phone OTP path (fallback: mobile → home phone → error)
  - `source=none` → Default path
- Contact verification checks occur before 2FA
- Password creation may be deferred to the end of the OPQ flow
- Can optionally fall through to the standard top-level flow via query parameters

---

## Technical Implementation Options (AFE Architecture)

Five options were evaluated for how to handle Profile/Contact creation without orphaned records:

| Option | Approach | Key Trade-off |
|---|---|---|
| **A** | Create PING User first → send OTP → create Profile/Contact after | Simpler flow; risk of orphaned PING users if OTP not completed |
| **B** | Add PING User to restricted access group during enrollment | Controlled access; more complex group management in DaVinci |
| **C** | Use SendGrid for email OTP (bypass PING for this step) | Decouples email from PING; adds SendGrid dependency |
| **D** | Create PING User + Profile normally; defer Contact creation until after validation | Clean Profile record; Contact only created post-verification |
| **E** | Same as D + DaVinci Flow changes + group management | Most complete; overlaps with Option B requirements; highest complexity |

> **Common pattern across options:** "Incomplete Enrollment" group in PING tracks users who have not finished enrollment, preventing partial account access.

---

---

## Edge Cases & Special Handling

| Scenario | Handling |
|---|---|
| **User already has an account** | Redirect to login flow |
| **NPS (Non-Purchasing Spouse)** | Must not reveal NPS status to the user during enrollment |
| **No email on record** | Error state — cannot proceed without email verification |
| **No phone for MFA** | Fall back to IDVQ |
| **Phone OTP fallback** | Mobile phone → Home phone → Error |
| **Orphaned Profile (no Contact)** | "Incomplete Enrollment" group tracks partial enrollments |
| **Rate limiting** | Enforced on OTP attempts to prevent brute-force attacks |
| **Magic link expiry** | Magic links should not live forever — expiration policy needed (open concern) |
| **Authorized users not on loan** | Handling TBD for users helping with the loan process but not listed as borrowers |

---

## Security Considerations

- **Account enumeration risk:** Without PING Protect, confirming whether an email is registered reveals account existence to attackers → MVP uses neutral/delayed responses
- **Rate limiting:** Required on all OTP delivery and verification endpoints
- **Orphaned records:** Partial enrollment must not leave accessible but incomplete accounts
- **Man-in-the-middle:** Email OTP + magic link security needs review
- **Forced password reset:** Separate MVP flow handles Okta → PING migration with forced password resets + SSN verification

---

## NOT MVP (Future Enhancements)

- PING Protect integration for instant email validation feedback
- Automated password creation prompts 24 hours post-enrollment
- Enhanced magic link functionality
- Voice OTP option
- Biometrics on secondary screens
- Device memory / "Remember Me" for biometrics
