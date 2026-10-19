# AuraCare Health — Modern AI-Powered Hospital Appointment Booking Platform

[![React](https://img.shields.io/badge/React-19-blue.svg)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue.svg)](https://www.typescriptlang.org/)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3.4-teal.svg)](https://tailwindcss.com/)
[![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-green.svg)](https://supabase.com/)
[![Google Gemini](https://img.shields.io/badge/Google-Gemini_AI-orange.svg)](https://ai.google.dev/)
[![Razorpay](https://img.shields.io/badge/Razorpay-Payment_Gateway-blue.svg)](https://razorpay.com/)

A production-oriented, responsive hospital appointment booking and patient care management web platform with a minimalist healthcare interface, double-booking prevention, encrypted patient records, AI-powered customer support, online payments, calendar integrations, and an operational admin dashboard.

---

## 1. Implemented Features

### A. Public Portal & Doctor Directory
- **Hero & Landing Page**: Headline *"Quality Healthcare, Made Easier."*, search bar, accreditation indicators, hospital departments, facilities, FAQs, and contact points.
- **Searchable Doctor Directory**: Multi-criteria filtering by clinical department, doctor name, qualifications, and consultation fee slider.
- **Doctor Profiles**: Detailed credentials, qualifications, experience, languages spoken, OPD room, fees, rating, and direct slot booking.

### B. Smart 8-Step Appointment Booking Wizard
1. **Step 1: Patient Details**: Full name, age/DOB, phone with country code (`+91`), email, gender, first-time vs returning patient with UHID, and 1-click profile auto-fill.
2. **Step 2: Describe Health Concern**: Quick selectors (Fever, Cough, Headache, Stomach pain, Joint pain, Skin, Eye, Accident/Injury, Follow-up), severity levels, free-text description, and consultation mode (In-Person OPD vs Telehealth Video).
   - **Emergency Detection**: High-priority alert banner appears immediately if acute or red-flag symptoms are chosen, instructing the patient to dial `112` or `108` and not wait for an appointment.
3. **Step 3: Department & Specialist Selection**: Automatic triage suggestion mapped to symptoms, department filter tabs, and specialist selection.
4. **Step 4: Dynamic Date & Time Slot Picker**: Real-time calendar respecting hospital timezone (`Asia/Kolkata`), 30-min duration slots, past time disabling, and **strict conflict prevention against double-booking**.
5. **Step 5: Previous Medical Records**: Prior hospital/doctor details and multi-file uploader (PDF, JPEG, PNG, max 10MB) with thumbnail preview, categorized tagging (prescription, lab report, scan), and private storage.
6. **Step 6: Review & Consent**: Comprehensive breakdown of patient info, date, time slot, consulting doctor, consultation fees, and mandatory consent to hospital policies & data processing.
7. **Step 7: Payment Flow**: Official Razorpay integration with support for UPI (GPay, PhonePe, Paytm), Cards, NetBanking, and an option to *Pay at Hospital Reception*.
8. **Step 8: Booking Confirmation**: Unique booking reference (`AURA-2026-XXXXX`), status badge, Add to Google Calendar web action, downloadable standard `.ics` calendar invitation, and official printable receipt modal.

### C. Gemini AI Healthcare Assistant
- Accessible floating widget on public & patient pages.
- Grounded in verified hospital departments, doctors, fees, and FAQs.
- Clinical safety guardrails: Never diagnoses or prescribes; immediately escalates life-threatening emergencies with ambulance hotlines.
- Secure backend proxy edge function (`supabase/functions/gemini-chat`) ensuring the Gemini API key remains server-side.

### D. Patient Dashboard
- Comprehensive view of upcoming and past consultations.
- Reschedule modal with real-time available slot picker.
- Cancellation modal with AuraCare's 4-hour cutoff policy and automated refund note.
- Medical Records Locker to view, download, and upload diagnostic files.
- Printable receipt viewer.

### E. Doctor Portal
- Dedicated attending physician view.
- Live patient queue for today's clinic and all assigned outpatients.
- Decrypted access to patient-uploaded medical reports.
- Clinical workspace: mark *Checked In*, *Completed*, record doctor consultation notes and Rx plan.

### F. Admin SaaS Dashboard
- Live database metrics: Total appointments, today's appointments, completed, cancelled, confirmed, and total revenue in INR.
- Visual volume distribution charts across departments and doctor utilization.
- Searchable & filterable appointment ledger with doctor reassignment, status changes, and destructive action controls.
- Complete Doctor CRUD management (add, edit, fee changes, activate/deactivate).
- 1-Click Export to **CSV** and **Excel-compatible XLSX** (via `xlsx`).
- Full immutable security audit trail logging every administrative and booking action.

---

## 2. Project Structure

```text
coffeeday/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── ai/
│   │   │   └── AIAssistant.tsx          # Floating Gemini assistant with triage safeguards
│   │   ├── booking/
│   │   │   ├── BookingWizard.tsx        # 8-Step state orchestrator & progress stepper
│   │   │   ├── Step1PatientDetails.tsx
│   │   │   ├── Step2HealthConcern.tsx
│   │   │   ├── Step3DoctorSelection.tsx
│   │   │   ├── Step4DateSlotPicker.tsx
│   │   │   ├── Step5MedicalRecords.tsx
│   │   │   ├── Step6ReviewConfirm.tsx
│   │   │   ├── Step7Payment.tsx
│   │   │   └── Step8Confirmation.tsx
│   │   └── common/
│   │       ├── EmergencyBanner.tsx      # Red-flag emergency alert component
│   │       ├── Footer.tsx               # Hospital accreditation & compliance footer
│   │       ├── Navbar.tsx               # Emergency hotline bar, navigation, role switcher
│   │       └── ReceiptModal.tsx         # Hospital printable & downloadable receipt
│   ├── lib/
│   │   ├── calendar.ts                  # RFC-5545 .ics generator & Google Calendar URLs
│   │   ├── emergencyRules.ts            # Triage keyword matcher & ambulance hotlines
│   │   ├── export.ts                    # CSV and XLSX export utilities
│   │   ├── mockData.ts                  # Initial seed dataset (doctors, depts, FAQs)
│   │   ├── storage.ts                   # Persistent store with double-booking prevention
│   │   └── supabase.ts                  # Supabase client & connection validator
│   ├── pages/
│   │   ├── AdminDashboard.tsx           # Full SaaS operations console & metrics
│   │   ├── AuthPage.tsx                 # Email, Google OAuth, and 1-click test roles
│   │   ├── DoctorDirectoryPage.tsx      # Searchable & filterable doctor directory
│   │   ├── DoctorPortal.tsx             # Doctor schedule & patient consultation portal
│   │   ├── HomePage.tsx                 # Public homepage (Hero, Depts, FAQs)
│   │   ├── PatientDashboard.tsx         # Patient appointments & documents locker
│   │   └── PrivacyTermsPage.tsx         # DPDP Act & Indian Medical Council terms
│   ├── services/
│   │   ├── appointmentService.ts        # Booking, conflict validation & queries
│   │   ├── authService.ts               # Auth, roles, profiles, and demo accounts
│   │   ├── departmentService.ts         # Departments and symptom suggestion triage
│   │   ├── doctorService.ts             # Doctor queries, schedules, and dynamic slots
│   │   ├── geminiService.ts             # AI chat proxy & local fallback engine
│   │   ├── paymentService.ts            # Razorpay order creation & signature check
│   │   └── storageService.ts            # Medical file validation & upload
│   ├── tests/
│   │   └── hospital.test.js             # Automated unit tests for core business logic
│   ├── types/
│   │   ├── database.ts                  # 17 PostgreSQL tables, enums, interfaces
│   │   └── index.ts                     # Wizard state, time slots, filters
│   ├── App.tsx                          # Root router & layout wrapper
│   ├── index.css                        # Tailwind styles, print sheet, scrollbar
│   └── main.tsx                         # Entry point
├── supabase/
│   ├── functions/
│   │   ├── gemini-chat/index.ts         # Edge Function: Secure Gemini AI chat
│   │   ├── google-calendar/index.ts     # Edge Function: Google Calendar sync
│   │   ├── razorpay-order/index.ts      # Edge Function: Razorpay Order Creation
│   │   └── razorpay-verify/index.ts     # Edge Function: Razorpay HMAC Verification
│   ├── migrations/
│   │   └── 20261019000001_hospital_schema.sql # 17 tables, RLS policies, trigger
│   └── seed.sql                         # Verified sample doctors, schedules, FAQs
├── .env.example                         # Documented public & server secrets template
├── package.json
├── tailwind.config.js
├── tsconfig.json
└── vite.config.js
```

---

## 3. Database Schema & Supabase SQL Setup

The database schema (`supabase/migrations/20261019000001_hospital_schema.sql`) implements all 17 required tables:
1. `profiles`
2. `user_roles`
3. `departments`
4. `doctors`
5. `doctor_departments`
6. `doctor_schedules`
7. `doctor_unavailability`
8. `patients`
9. `appointments` (includes PostgreSQL trigger `check_doctor_appointment_overlap` to guarantee double-booking prevention)
10. `appointment_status_history`
11. `medical_documents`
12. `payments`
13. `notifications`
14. `hospital_settings`
15. `faqs`
16. `google_calendar_events`
17. `audit_logs`

### Running the Database Migration
In your Supabase project dashboard:
1. Navigate to the **SQL Editor**.
2. Copy and paste the contents of `supabase/migrations/20261019000001_hospital_schema.sql` and click **Run**.
3. Copy and paste the contents of `supabase/seed.sql` and click **Run**.
4. In **Storage**, create a private bucket named `medical-records`.

---

## 4. Environment Variables

Create `.env` based on `.env.example`:

```bash
# Public variables (Client-side in browser)
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
VITE_RAZORPAY_KEY_ID=rzp_test_yourKeyIdHere

# Server-only secrets (Never expose to browser; configure in Supabase Edge Functions)
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
GEMINI_API_KEY=AIzaSy...
RAZORPAY_KEY_SECRET=your_razorpay_secret...
RAZORPAY_WEBHOOK_SECRET=your_razorpay_webhook_secret...
GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=GOCSPX-...
```

---

## 5. Local Setup & Execution

### Prerequisites
- Node.js `v20+` or `v24+`
- npm `v10+`

### Installation
```bash
# Install dependencies
npm.cmd install

# Run automated test suite
npm.cmd test

# Build for production
npm.cmd run build

# Start local development server
npm.cmd run dev
```

Visit `http://localhost:5173` in your browser.

---

## 6. Testing Results & Scenarios Covered

Run the automated test suite with:
```bash
npm.cmd test
```

### Verified Test Cases:
- ✔ **Emergency Triage**: Detects acute chest pain, heart attacks, accident trauma, and slurred speech, triggering high-severity emergency ambulance actions (`112 / 108`).
- ✔ **Department Suggestion**: Correctly maps joint symptoms to Orthopedics, heart concerns to Cardiology, infant symptoms to Pediatrics, and skin concerns to Dermatology.
- ✔ **Scheduling Conflict Protection**: Prevents duplicate slot bookings for the same doctor with exact collision and partial overlap detection.
- ✔ **Medical Document Storage**: Enforces allowed MIME types (`application/pdf`, `image/jpeg`, `image/png`) and rejects files exceeding 10MB.
- ✔ **iCalendar RFC-5545 Generation**: Emits RFC-compliant `.ics` payloads with `UID`, `STATUS:CONFIRMED`, and alarm triggers.

### Testing Account Switcher:
You can test any role immediately from the Navbar role switcher or Auth modal:
- **Patient**: Ramesh Patel (`ramesh.patel@example.com`)
- **Doctor**: Dr. Rajesh Iyer (`dr.rajesh@auracarehealth.org`)
- **Admin**: Chief Admin Dr. Sarah Thomas (`admin@auracarehealth.org`)

---

## 7. Production Deployment (Vercel + Supabase)

1. **Deploy Supabase Edge Functions**:
   ```bash
   npx supabase functions deploy gemini-chat
   npx supabase functions deploy razorpay-order
   npx supabase functions deploy razorpay-verify
   npx supabase functions deploy google-calendar
   ```
2. **Deploy to Vercel**:
   - Push repository to GitHub/GitLab.
   - Import project into Vercel.
   - Set Build Command: `vite build`
   - Set Output Directory: `dist`
   - Add environment variables: `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, `VITE_RAZORPAY_KEY_ID`.
3. Click **Deploy**.