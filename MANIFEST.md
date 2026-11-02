# Bolo Bazar — Complete File Manifest

> Generated: November 2026 | Phase 1 + Phase 2 complete deliverable.

This manifest catalogs every file in the `Bolo-Bazar/` project folder, its purpose, and origin.

---

## Root Files

| File | Purpose | Origin |
|------|---------|--------|
| `README.md` | Full project overview, quick start guide, KYC architecture, demo personas, test suite | Authored |
| `MANIFEST.md` | This file — complete file catalog | Generated |
| `IMPLEMENTATION_STATUS.md` | Feature implementation status tracker | Generated |
| `.gitignore` | Git exclusion rules for secrets, build artifacts, databases | Generated |

---

## Backend (`backend/`)

### Entry Point

| File | Purpose |
|------|---------|
| `backend/start.ps1` | PowerShell script to start the FastAPI server (sets PYTHONUTF8, PYTHONPATH, uvicorn) |
| `backend/requirements.txt` | Python package dependencies (FastAPI, uvicorn, SQLAlchemy, Pydantic, bcrypt, cryptography, pytest) |
| `backend/.env.example` | Environment variable template with all required keys (JWT secret, encryption key, provider mode, etc.) |

### Application (`backend/app/`)

| File | Purpose |
|------|---------|
| `app/__init__.py` | Package marker (empty) |
| `app/main.py` | FastAPI app factory, CORS configuration, router mounting, lifespan handler (DB init) |

### API Layer (`backend/app/api/`)

| File | Purpose |
|------|---------|
| `api/__init__.py` | Package marker (empty) |
| `api/auth.py` | Auth endpoints: registration, login, OTP send/verify, token refresh, logout |
| `api/kyc.py` | KYC endpoints: Aadhaar verify, face verify, address save, bank verify, submit application |
| `api/admin.py` | Admin-only endpoints: KYC queue, application detail, approve/reject, audit logs |
| `api/health.py` | Health check endpoint (`GET /api/health`) — returns service status and version |
| `api/deps.py` | FastAPI dependency injection: JWT auth guard, role checker, DB session provider |

### Core Layer (`backend/app/core/`)

| File | Purpose |
|------|---------|
| `core/__init__.py` | Package marker (empty) |
| `core/config.py` | Pydantic `Settings` class loading all environment variables with validation |
| `core/database.py` | SQLAlchemy engine, `SessionLocal` factory, `Base` metadata, `init_db()` seeder |
| `core/security.py` | Bcrypt password hashing (cost 12), JWT signing/verification, AES-256 Fernet encryption/decryption |

### Models (`backend/app/models/`)

| File | Purpose |
|------|---------|
| `models/__init__.py` | Package marker (empty) |
| `models/user.py` | `User` SQLAlchemy model — phone, hashed_password, role, KYC status, timestamps |
| `models/verification.py` | `KYCApplication`, `IdentityVerification`, `FaceVerification`, `BankAccount`, `AuditLog`, `ConsentRecord` models |

### Schemas (`backend/app/schemas/`)

| File | Purpose |
|------|---------|
| `schemas/__init__.py` | Package marker (empty) |
| `schemas/auth.py` | Pydantic schemas for auth flows: `RegisterRequest`, `LoginRequest`, `OTPRequest`, `TokenResponse` |
| `schemas/kyc.py` | Pydantic schemas for KYC flows: `AadhaarVerifyRequest`, `FaceVerifyRequest`, `BankVerifyRequest`, `KYCSubmitRequest` |

### Services (`backend/app/services/`)

| File | Purpose |
|------|---------|
| `services/__init__.py` | Package marker (empty) |
| `services/sms_provider.py` | SMS gateway factory — `MockSandboxSMSProvider` (OTP=`123456`) / `ProductionSMSProvider` (Twilio/Fast2SMS) |
| `services/kyc_provider.py` | Aadhaar/DigiLocker OVSE factory — Sandbox XML simulation / Production (Signzy/IDfy) |
| `services/face_provider.py` | Liveness + 1:1 face match factory — Sandbox challenge engine / Production (HyperVerge/Rekognition) |
| `services/bank_provider.py` | IMPS Penny-Drop + IFSC validation factory — Sandbox simulator / Production (Razorpay/Cashfree) |
| `services/audit_service.py` | Tamper-evident append-only audit log writer for all admin/user verification actions |

### Tests (`backend/tests/`)

| File | Purpose |
|------|---------|
| `tests/__init__.py` | Package marker (empty) |
| `tests/test_phase2_kyc.py` | 17-test pytest suite covering security, crypto, validation, RBAC, OTP expiry, Aadhaar masking |

---

## Frontend (`frontend/`)

### Config Files

| File | Purpose |
|------|---------|
| `index.html` | Vite entry point HTML with font preloads (DM Serif Display, Poppins, Noto Sans Devanagari) |
| `package.json` | Node.js package manifest (React 18, Vite 6, Tailwind CSS 3, React Router v6) |
| `vite.config.js` | Vite config with React plugin and dev server proxy to backend port 8000 |
| `tailwind.config.js` | Tailwind config with custom Bolo Bazar color tokens (Blue, Butter, Chocopie) |
| `postcss.config.js` | PostCSS config for Tailwind CSS and Autoprefixer |
| `public/manifest.json` | PWA manifest — name, icons, theme colors, display mode, language (hi-IN) |

### Source Root (`frontend/src/`)

| File | Purpose |
|------|---------|
| `src/main.jsx` | React DOM render root, wraps app in `AuthProvider` and `LanguageProvider` |
| `src/App.jsx` | React Router route definitions — all pages and layouts wired up |
| `src/index.css` | Global CSS, Tailwind directives, custom scrollbar and animation utilities |

### Components — Common (`frontend/src/components/common/`)

| File | Purpose |
|------|---------|
| `AuthModal.jsx` | Animated login/register modal with phone+password form and OTP verification step |
| `Header.jsx` | Top navigation bar with language switcher, persona selector, auth state, and trust badge |
| `VerifiedBadge.jsx` | Animated green/yellow/grey KYC status badge component with tooltip |

### Components — KYC (`frontend/src/components/kyc/`)

| File | Purpose |
|------|---------|
| `KYCModal.jsx` | Full 7-step interactive KYC wizard: Basic Info → OTP → Aadhaar → Selfie → Address → Bank → Review & Submit |

### Context (`frontend/src/context/`)

| File | Purpose |
|------|---------|
| `AuthContext.jsx` | Global auth state (user, token, login/logout), JWT decode, role detection |
| `LanguageContext.jsx` | Hindi/English bilingual string dictionary and language switcher context |

### Layouts (`frontend/src/layouts/`)

| File | Purpose |
|------|---------|
| `RootLayout.jsx` | Root layout with Header, Outlet, and global notification provider |
| `SellerLayout.jsx` | Seller dashboard sidebar layout with nav links (Dashboard, Add Product, Orders, Profile) |
| `ShopLayout.jsx` | Public-facing shop layout (minimal header, back to store link) |
| `AdminLayout.jsx` | Admin panel layout with KYC queue and security log navigation |

### Pages (`frontend/src/pages/`)

| File | Purpose |
|------|---------|
| `LandingPage.jsx` | Marketing hero page with animated feature highlights and CTA for seller onboarding |
| `admin/AdminDashboard.jsx` | KYC moderation queue with application inspector, approve/reject buttons, and audit log viewer |
| `seller/SellerDashboard.jsx` | Seller home: KYC status banner, earnings summary, recent orders, quick actions |
| `seller/AddProductFlow.jsx` | Voice-to-product flow: mic capture, AI description, background removal, pricing, publish |
| `seller/SellerOrders.jsx` | Seller orders list with status filters (pending/delivered/cancelled), WhatsApp action |
| `seller/SellerProfile.jsx` | Profile editor with profile completion bar, KYC trigger button, bank details |
| `shop/ShopHome.jsx` | Public store catalog — product grid with search, category filters |
| `shop/ProductDetail.jsx` | Individual product page with full description, seller trust score, WhatsApp order button |
| `shop/PublicStore.jsx` | Branded mini-store for sharing — QR code, copy link, WhatsApp share |

---

## Docs (`docs/`)

| File | Purpose |
|------|---------|
| `docs/ARCHITECTURE.md` | Detailed cybersecurity and compliance architecture: threat model, DB schema, provider abstraction, Mermaid flow |

---

**Total tracked files: 57**
