# Combined OTP Experience

Source: [Miro Board — Combined OTP Experience](https://miro.com/app/board/uXjVKOofDy0=/?moveToWidget=3458764653979222307)

## Overview

The Combined OTP Experience defines the complete authentication decision matrix for all user login and verification flows. It maps every combination of user profile type, password status, and registered device state to a specific authentication path — covering the **first factor**, **MFA (second factor)**, and **Sign In Another Way (SIAW)** fallback options at each stage.

---

## Matrix Dimensions

| Dimension | Values |
|---|---|
| **Profile Type** | Lead Only Profile · Has Contact |
| **Password State** | Has Password · No Password |
| **Device State (PING)** | No Devices · Email Device Only · Has One Phone + Email · Has Multiple Phones + Email |
| **Product Variant** | Non-OPQ · OPQ · AFE |

---

## Authentication Steps (Rows)

| Row | Step |
|---|---|
| 1 | **First Factor** — Primary authentication method |
| 2 | **Sign In Another Way (First Factor)** — Fallback if user can't complete first factor |
| 3 | **MFA if First Factor = Password** — Second factor after password login |
| 4 | **SIAW if First Factor = Password** — Fallback at MFA stage after password |
| 5 | **MFA if First Factor = Phone** — Second factor after phone device OTP |
| 6 | **SIAW if First Factor = Phone** — Fallback at MFA stage after phone OTP |
| 7 | **MFA if First Factor = Email** — Second factor after email device OTP |
| 8 | **SIAW if First Factor = Email** — Fallback at MFA stage after email OTP |

> **SIAW** = Sign In Another Way. The "I can't use this method" escape hatch shown at each authentication stage.

---

## Authentication Method Glossary

| Term | Definition |
|---|---|
| **Password** | User's standard account password |
| **Email Device** | Email address registered as a PING MFA device |
| **Phone Device** | Phone number registered as a PING MFA device |
| **Profile Email** | Email from the user's lead profile (not a PING device) |
| **Contact Email** | Email from the linked contact record |
| **Mobile Phone From Contact** | Phone number sourced from the contact record |
| **Last Created Phone Device** | Most recently registered phone device when multiple exist |
| **Matching Lead Phone ELSE Email** | OPQ logic: match phone from lead; fall back to email if no match |
| **Matching Lead Phone** | OPQ logic: phone number that matches the lead record |
| **IDVQ** | Identity Verification Questions — final fallback in Has Contact scenarios |
| **N/A** | Step not applicable for this scenario combination |
| **Non-OPQ** | Standard authentication flow |
| **OPQ** | Optimized flow; pre-identifies user, uses Matching Lead Phone / Mobile Phone From Contact logic |
| **AFE** | Account First Enrollment — alternate entry flow for new account creation |

---

## All Use Cases

---

### SCENARIO 1 — Lead Only Profile | Has Password | No Devices

> No PING phone device, no PING email device. Password exists. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Profile Email | Profile Email | Profile Email |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 2 — Lead Only Profile | Has Password | Email Device Only

> PING email device registered. No phone device. Password exists. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Email Device | Email Device | Email Device |
| MFA if First Factor = Password | Email Device | Email Device | Email Device |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 3 — Lead Only Profile | Has Password | Has One Phone + Email

> One PING phone device + PING email device registered. Password exists. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Phone Device | Phone Device | Phone Device |
| MFA if First Factor = Password | Phone Device | Phone Device | Phone Device |
| SIAW if First Factor = Password | Email Device | Email Device | Email Device |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | Phone Device | Phone Device | Phone Device |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 4 — Lead Only Profile | Has Password | Has Multiple Phones + Email

> Multiple PING phone devices + PING email device. Password exists. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Last Created Phone Device | Last Created Phone Device | Last Created Phone Device |
| MFA if First Factor = Password | Last Created Phone Device | Last Created Phone Device | Last Created Phone Device |
| SIAW if First Factor = Password | Email Device | Email Device | Email Device |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | Last Created Phone Device | Last Created Phone Device | Last Created Phone Device |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 5 — Lead Only Profile | No Password | No Devices

> No password. No PING devices. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Profile Email | Profile Email | Profile Email |
| SIAW (First Factor) | N/A | N/A | N/A |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 6 — Lead Only Profile | No Password | Email Device Only

> No password. PING email device only. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Email Device | Email Device | Email Device |
| SIAW (First Factor) | N/A | N/A | N/A |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 7 — Lead Only Profile | No Password | Has One Phone + Email

> No password. One PING phone device + email device. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Phone Device | Phone Device | Phone Device |
| SIAW (First Factor) | Email Device | Email Device | Email Device |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | Phone Device | Phone Device | Phone Device |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 8 — Lead Only Profile | No Password | Has Multiple Phones + Email

> No password. Multiple PING phone devices + email device. No contact record.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Last Created Phone Device | Last Created Phone Device | Last Created Phone Device |
| SIAW (First Factor) | Email Device | Email Device | Email Device |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | Last Created Phone Device | Last Created Phone Device | Last Created Phone Device |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 9 — Has Contact | Has Password | No Devices

> No PING devices. Password exists. Full contact record available (Contact Email, Mobile Phone From Contact).

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Contact Email | Mobile Phone From Contact | Contact Email |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | IDVQ | IDVQ | IDVQ |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | N/A | N/A | N/A |

---

### SCENARIO 10 — Has Contact | Has Password | Email Device Only

> PING email device registered. No phone device. Password exists. Contact record available.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Email Device | Email Device | Email Device |
| MFA if First Factor = Password | Email Device | Email Device | Email Device |
| SIAW if First Factor = Password | IDVQ | IDVQ | IDVQ |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

### SCENARIO 11 — Has Contact | Has Password | Has One Phone + Email

> One PING phone device + email device. Password exists. Contact record available.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Phone Device | Mobile Phone From Contact | Phone Device |
| MFA if First Factor = Password | Phone Device | Mobile Phone From Contact | Phone Device |
| SIAW if First Factor = Password | Email Device | Email Device | Email Device |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | IDVQ | IDVQ | IDVQ |
| MFA if First Factor = Email | Phone Device | Mobile Phone From Contact | Phone Device |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

### SCENARIO 12 — Has Contact | Has Password | Has Multiple Phones + Email

> Multiple PING phone devices + email device. Password exists. Contact record available.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Password | Password | Password |
| SIAW (First Factor) | Last Created Phone Device | Matching Lead Phone ELSE Email | Last Created Phone Device |
| MFA if First Factor = Password | Last Created Phone Device | Matching Lead Phone ELSE Email | Last Created Phone Device |
| SIAW if First Factor = Password | Email Device | Email Device | Email Device |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | IDVQ | IDVQ | IDVQ |
| MFA if First Factor = Email | Last Created Phone Device | Matching Lead Phone | Last Created Phone Device |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

### SCENARIO 13 — Has Contact | No Password | No Devices

> No password. No PING devices. Contact record available (Contact Email, Mobile Phone From Contact).

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Contact Email | Mobile Phone From Contact | Contact Email |
| SIAW (First Factor) | IDVQ | Contact Email | IDVQ |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

### SCENARIO 14 — Has Contact | No Password | Email Device Only

> No password. PING email device only. Contact record available.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Email Device | Email Device | Email Device |
| SIAW (First Factor) | Contact Email | Mobile Phone From Contact | Contact Email |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | N/A | N/A | N/A |
| SIAW if First Factor = Phone | N/A | N/A | N/A |
| MFA if First Factor = Email | N/A | N/A | N/A |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

### SCENARIO 15 — Has Contact | No Password | Has One Phone + Email

> No password. One PING phone device + email device. Contact record available.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Phone Device | Mobile Phone From Contact | Phone Device |
| SIAW (First Factor) | Email Device | Phone Device | Email Device |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | IDVQ | IDVQ | IDVQ |
| MFA if First Factor = Email | Phone Device | Mobile Phone From Contact | Phone Device |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

### SCENARIO 16 — Has Contact | No Password | Has Multiple Phones + Email

> No password. Multiple PING phone devices + email device. Contact record available.

| Auth Step | Non-OPQ | OPQ | AFE |
|---|---|---|---|
| First Factor | Last Created Phone Device | Matching Lead Phone ELSE Email | Last Created Phone Device |
| SIAW (First Factor) | Email Device | Phone Device | Email Device |
| MFA if First Factor = Password | N/A | N/A | N/A |
| SIAW if First Factor = Password | N/A | N/A | N/A |
| MFA if First Factor = Phone | Email Device | Email Device | Email Device |
| SIAW if First Factor = Phone | IDVQ | IDVQ | IDVQ |
| MFA if First Factor = Email | Last Created Phone Device | Matching Lead Phone | Last Created Phone Device |
| SIAW if First Factor = Email | IDVQ | IDVQ | IDVQ |

---

## Key Business Rules

1. **First Factor Priority (passwordless):** Password → Last Used Phone Device → Email Device → Profile/Contact Email → Mobile Phone From Contact
2. **Last Created Phone:** When multiple phone devices exist, default to the most recently registered one
3. **Matching Lead Phone (OPQ):** Match phone number from lead record; fall back to email if no match
4. **Mobile Phone From Contact (OPQ):** In OPQ flows with a Contact profile, phone is sourced directly from the contact record
5. **IDVQ Fallback:** Identity Verification Questions surface only in Has Contact scenarios when device-based MFA cannot be satisfied
6. **Lead Only Limitation:** Lead-only profiles cannot use Contact Email or Mobile Phone From Contact — those sources require a contact record
7. **OPQ vs Non-OPQ:** OPQ flows may mark certain MFA steps N/A where Non-OPQ would require them, and use contact-sourced devices as the preferred option
8. **N/A Entries:** A step is N/A when it is structurally not applicable — e.g., no MFA is possible when no PING devices are registered, or "MFA if First Factor = Phone" is N/A when no phone device exists
9. **Profile Email:** Always sourced from the existing lead profile, never from a newly entered email address during login
10. **Email vs Phone MFA:** When both devices exist, phone is preferred as MFA over email
