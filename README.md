# Bolo Bazar (बोलो बाज़ार) 🇮🇳
> *"Speak. Snap. Share."* — Voice-first marketplace empowering women micro-entrepreneurs across India.

[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688.svg?style=flat&logo=fastapi)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18.3-61DAFB.svg?style=flat&logo=react)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-6.0-646CFF.svg?style=flat&logo=vite)](https://vitejs.dev)
[![Tailwind CSS](https://img.shields.io/badge/TailwindCSS-3.4-38B2AC.svg?style=flat&logo=tailwind-css)](https://tailwindcss.com)
[![DPDP Act 2023](https://img.shields.io/badge/DPDP%20Act%202023-Compliant-success.svg)](https://www.meity.gov.in)
[![Tests](https://img.shields.io/badge/Pytest-17%20Passed-brightgreen.svg)]()

---

## 🌟 Overview

**Bolo Bazar** connects home-based women sellers (artisans, pickle makers, tailors, bakers) with buyers through high-trust, voice-first commerce:
1. **Speak**: The seller describes her craft by speaking in Hindi (`hi-IN`) or English (`en-IN`).
2. **Snap**: She uploads a photo, and the AI cleans the background automatically.
3. **Share**: A branded mini-store is generated with a one-tap WhatsApp order and share link.

### 🛡️ Phase 2: Advanced Security, KYC & Identity Verification
Phase 2 establishes fintech-grade security and identity trust tailored for Indian micro-merchants while adhering strictly to the **Digital Personal Data Protection (DPDP) Act 2023** and **UIDAI Offline e-KYC Guidelines**:
- **Zero Raw Aadhaar Storage**: Complete 12-digit Aadhaar numbers and biometric templates are NEVER stored. Only SHA-256 salted hashes and masked identifiers (`XXXX-XXXX-8910`) are preserved.
- **AES-256 Fernet Encryption**: Sensitive bank account numbers and PII fields are encrypted at rest.
- **Interactive 7-Step Verification**: Phone OTP, Aadhaar/DigiLocker OVSE, live selfie anti-spoofing challenge with 1:1 face matching, pickup address validation, and IMPS Penny-Drop bank verification.
- **Admin Moderation & Audit System**: Real-time review queue, application inspector, approval/rejection workflows, and tamper-evident audit logs.

---

## 🎨 Design System & Accessibility

Built to WCAG AA contrast standards with large tap targets (minimum 48px) and warm Indian aesthetics:

| Token | Hex | Role | Contrast Ratio |
|---|---|---|---|
| **Blue** | `#C4DAE8` | Calm surfaces, background | Baseline surface |
| **Butter** | `#FEEFB8` | Action cards, buttons, badges | 12.8:1 with Chocopie |
| **Chocopie** | `#432F2E` | Deep headings, readable text | 9.4:1 with Blue |

- **Typography**: `DM Serif Display` (headings), `Poppins` (body text), and `Noto Sans Devanagari` (Hindi text).
- **Audio Feedback**: Text-to-speech prompts guide first-time smartphone users through every step.

---

## 🔒 7-Step User KYC Architecture

```
[ Step 1: Basic Info ] ────────► Full Name, Mobile, Email, DOB
       │
[ Step 2: Mobile OTP ] ────────► +91 Verification (SMS Provider sandbox/live, 5-min expiry, rate limited)
       │
[ Step 3: Govt ID ]    ────────► DigiLocker / Aadhaar OVSE (Masked ID, Name Match, Zero raw Aadhaar)
       │
[ Step 4: Live Selfie ] ───────► Camera Stream + Anti-Spoofing Challenge + 1:1 ID Face Matching
       │
[ Step 5: Address ]    ────────► Pickup Address, Indian PIN code validation (6 digits)
       │
[ Step 6: Bank & Penny]────────► IFSC Validation + IMPS Penny Drop + AES-256 Account Encryption
       │
[ Step 7: Final Review]────────► DPDP Consent Signature, Summary Inspection, Application Submission
       │
   [ ADMIN ]           ────────► Moderation Queue: Approve / Reject / Audit Log Inspection
```

---

## 🚀 Quick Start Guide

### Prerequisites
- **Python**: 3.10+ (Self-contained in `scratch\tools\python\python.exe`)
- **Node.js**: v18+ (Self-contained in `scratch\tools\node\node.exe`)

---

### 1. Run the Backend (FastAPI)

```powershell
# Navigate to backend directory
cd backend

# Run automated test suite (17/17 tests must pass)
& "..\tools\python\python.exe" -m pytest -v tests/

# Start the FastAPI server on port 8000
$env:PYTHONUTF8 = "1"
$env:PYTHONPATH = (Get-Location).Path
& "..\tools\python\python.exe" -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

- **Interactive API Docs (Swagger)**: [http://localhost:8000/docs](http://localhost:8000/docs)
- **API Health Check**: [http://localhost:8000/api/health](http://localhost:8000/api/health)

---

### 2. Run the Frontend (React + Vite)

```powershell
# In a new terminal, navigate to frontend directory
cd frontend

# Start the Vite development server
$env:PATH = "..\tools\node;" + $env:PATH
& "..\tools\node\node.exe" "..\tools\node\node_modules\npm\bin\npm-cli.js" run dev
```

- **Open in Browser**: [http://localhost:5173](http://localhost:5173)

---

## 🔑 Pre-Configured Demo Personas

Switch between personas with 1-click in the app header:

| Persona | Role | Phone | Password / Test OTP | KYC Status | What to Test |
|---|---|---|---|---|---|
| **Sunita Sharma** | Verified Seller | `9876543210` | `Seller@1234` / `123456` | **VERIFIED** | Green Trust Badge, Voice Add Product, Public Store |
| **Priya Kumari** | New Seller | `9811223344` | `Seller@1234` / `123456` | **PENDING** | Profile Completion Bar, 7-Step KYC Interactive Modal |
| **Bolo Bazar Admin** | Platform Admin | `9999900001` | `Admin@1234` / `123456` | **ADMIN** | KYC Moderation Queue, Approve/Reject, Security Logs |

---

## 🧪 Security & Compliance Verification

The automated test suite (`tests/test_phase2_kyc.py`) validates:
1. `test_valid_registration_phone_and_password` — Bcrypt hashing & credential validation.
2. `test_invalid_phone_numbers` — Strict Indian phone validation (`^[6-9]\d{9}$`).
3. `test_incorrect_otp_rejection` — 6-digit OTP verification with failure attempts counter.
4. `test_expired_otp_detection` — Time-windowed expiration enforcement.
5. `test_otp_cooldown_calculation` — Rate-limiting to prevent SMS flood attacks.
6. `test_aadhaar_masking_and_hashing` — Verifies raw Aadhaar is never saved, only masked `XXXX-XXXX-8910`.
7. `test_face_liveness_anti_spoofing` — Detects camera spoofing and synthetic replays.
8. `test_ifsc_validation` — RBI 11-character alphanumeric IFSC format check.
9. `test_bank_account_encryption_at_rest` — AES-256 Fernet ciphertext verification at rest.
10. `test_admin_role_authorization` — Role-based access control (RBAC) protecting moderation routes.

---

## 📁 Repository Structure

```
bolo-bazar/
├── backend/
│   ├── app/
│   │   ├── api/          # Routers: auth, kyc, admin, health
│   │   ├── core/         # Security, AES-256 Fernet, DB engine, config
│   │   ├── models/       # SQLAlchemy models (User, KYCApplication, AuditLog)
│   │   ├── schemas/      # Strict Pydantic models for validation
│   │   └── services/     # Providers (DigiLocker OVSE, SMS, Penny Drop, Face)
│   ├── tests/            # Pytest test suite (17 comprehensive tests)
│   ├── .env.example      # Environment variables template
│   └── requirements.txt  # Python dependencies
├── frontend/
│   ├── src/
│   │   ├── components/   # KYCModal (7-step flow), AuthModal, VerifiedBadge, Header
│   │   ├── context/      # AuthContext, LanguageContext (Hindi/English)
│   │   ├── layouts/      # RootLayout, SellerLayout, ShopLayout, AdminLayout
│   │   └── pages/        # LandingPage, ShopHome, ProductDetail, PublicStore,
│   │                     # SellerDashboard, AddProductFlow, SellerOrders, SellerProfile, AdminDashboard
│   └── package.json
└── docs/
    └── ARCHITECTURE.md   # Detailed cybersecurity & compliance architectural guide
```

---

## 📜 Regulatory & Legal Compliance
- **Digital Personal Data Protection (DPDP) Act 2023**: Explicit consent logging, data minimization, right to erasure, and zero raw storage of biometric identifiers.
- **UIDAI Offline e-KYC Guidelines**: Aadhaar numbers masked to last 4 digits everywhere. No paper copy or raw XML storage.
- **Reserve Bank of India (RBI)**: Account validation executed via authorized IMPS penny-drop verification without requiring ATM PINs or banking credentials.
