# KOGI INVEST ONE-STOP CENTRE (KIOSC v1.2)
### Official Single-Window Investment Facilitation Web Application

[![Vercel Deployment Ready](https://img.shields.io/badge/Deployment-Vercel%20Ready-black?style=flat&logo=vercel)](https://vercel.com)
[![Kogi State Government](https://img.shields.io/badge/Kogi%20State-Government%20Gateway-16845B?style=flat)](#)
[![TypeScript](https://img.shields.io/badge/Language-TypeScript%20React-blue?style=flat&logo=typescript)](https://www.typescriptlang.org/)
[![Tailwind CSS v4](https://img.shields.io/badge/Tailwind%20CSS-v4-38B2AC?style=flat&logo=tailwind-css)](https://tailwindcss.com/)

---

## 🏛️ Executive Summary & Coordination Policy

**KIOSC coordinates government processes; it does not itself grant land, licences, tax relief, permits, funding or statutory approval.** Substantive approvals remain the constitutional mandate of respective Ministries, Departments, and Agencies (MDAs).

- **Source of Truth**: Official KIOSC v1.2 Specification.
- **Visual Identity**: Official public-sector design inspired by Confluence State architecture: Primary Navy `#102A43`, Secondary Navy `#173B5E`, Kogi Green `#16845B`, Gold `#D6A62E`, Background `#F5F7FA`, Slate `#334155`.
- **System Time Standard**: `Africa/Lagos` (WAT, UTC+1).
- **Compliance Rules**: No fake statistics, no unconfirmed fees, no SMS/WhatsApp automation, no anonymous lookup, payments default disabled pending gazette.

---

## 🚀 Quick Start & Local Setup

```bash
# Clone the repository
git clone <repo-url>
cd KIOSC

# Install project dependencies
npm install

# Start the local development server (Vite)
npm run dev

# Build the production bundle for Vercel
npm run build
```

The application runs locally on `http://localhost:5173`.

---

## 🧭 Application Routes & Views Map

### 1. Public Website
- `/` or `Home`: Hero banner (*"INVEST IN KOGI. BUILD FROM THE CENTER."*), 5-Step Frictionless Methodology, 6 Institutional Gateway Cards, Statutory Procedures & Permit Guidelines Center, Priority Growth Sectors, and Confluence Geo-Zones Map.
- `Services`: Unified gateway to accelerated approvals (Land, Compliance, Infrastructure, Advisory, Aftercare, GRM).
- `How It Works`: 5-Step digitized pipeline overview from intake to inception.
- `Procedures Guide`: Dedicated in-browser reader for official state handbooks (C of O, Business Premises, Building Permit, EIA, Solid Minerals, Tax Clearance).
- `Key Sectors`: Interactive exploration of 8 priority sectors (Agriculture, Mining, Energy, Infrastructure, Waterways, Tourism, Manufacturing, Tech Hubs).
- `Investment Map / Geo-Zones`: Interactive spatial breakdown of Kogi Central, West, and East with LGA endowments and list fallback.
- `Opportunities`: Searchable and filterable investment dossiers.
- `Policies & Incentives`: Statutory laws, Pioneer Status (PSI), and land titling frameworks.
- `Aftercare`: Lifecycle investor concierge desk and mandatory Single PDF progress reporting protocol.
- `GRM / Help`: Grievance Redress Mechanism guide, eligible dispute categories, and general enquiry form.
- `About / Contact`: KIPA structure, participating MDA desk directory, and physical complex coordinates in Lokoja.

### 2. Authentication Screens
- `Sign In`: Single entry point with tabs for Investor Portal vs Authorized Government Staff, quick demo account presets, and session-expired notification.
- `Register`: 2-Step investor onboarding (Account credentials + Enterprise profile). Note: initial permitted use is not blocked pending email confirmation; emails are never falsely shown as verified.
- `Forgot Password`: Corporate email reset token dispatch modal.
- `Access Denied (403)`: Standard restricted-access screen for unauthorized role attempts.

### 3. Role-Based Workspaces
The application implements all 6 official specification roles:

| Role Category | Capabilities / Permitted Scope | Landing Screen |
| :--- | :--- | :--- |
| **1. Investor** | Corporate profile, 10-step wizard, document vault, timeline, messages, Aftercare, GRM. | `/investor-portal` |
| **2. Investment Agency** | Officer (Intake triage, routing, queries), Manager (Allocation, escalation, decision), Observer (Read-only auditor). | `/agency-workspace` |
| **3. MDA Desk** | Scoped to specific MDA (e.g., KOGIS Lands, KGIRS, Solid Minerals, Commerce, Environment, Works). Technical checklist, findings, report upload, concurrence. | `/mda-workspace` |
| **4. KGIRS / Payment Ops** | Fee catalogue audit, proof verification queue (*"Proof submitted for verification"* vs *"Payment verified"*). | `/payment-operations` |
| **5. Platform & ICT Admin** | User directories, role provisioning, health telemetry. | `/admin-portal` |
| **6. Superadmin** | Dynamic form builder, workflow stage config, SLA rules, anti-lockout protection, feature flags. | `/admin-portal` |

---

## 🛠️ Architecture & Backend Hand-off Contract (Phase 2 - Antigravity)

The frontend is architected with a decoupled data layer (`src/services/dataService.ts`) operating over a local/in-memory development adapter (`src/services/mockData.ts`). All data records are clearly labeled `DEMO` and isolated.

### Supabase Integration Architecture
The client initialization file is located at `src/services/supabaseClient.ts`. In Phase 2, Antigravity will connect the real Supabase backend without altering UI components.

#### 1. Expected Database Tables
- `profiles`: `id`, `full_name`, `email`, `phone_number`, `role`, `agency_capability`, `mda_capability`, `mda_department_id`, `company_name`, `company_rc_number`, `is_email_verified`, `created_at`
- `applications`: `id`, `reference_number`, `investor_id`, `project_title`, `project_objectives`, `business_model_summary`, `sector`, `sub_sector`, `investment_amount`, `currency`, `funding_source`, `expected_direct_jobs`, `expected_indirect_jobs`, `geo_zone`, `lga`, `proposed_site_address`, `land_requirement_acres`, `environmental_screening_score`, `status`, `priority`, `created_at`
- `application_documents`: `id`, `application_id`, `category_id`, `category_title`, `file_name`, `file_url`, `verified_by_staff`, `uploaded_at`
- `application_messages`: `id`, `application_id`, `sender_id`, `sender_name`, `sender_role`, `is_internal_only`, `content`, `timestamp`
- `mda_routings`: `id`, `application_id`, `mda_id`, `mda_name`, `status`, `assigned_officer_name`, `due_date`, `findings`, `recommendation`, `reviewed_by_manager`
- `grievances`: `id`, `ticket_ref`, `investor_id`, `category`, `mda_concerned`, `subject`, `detailed_description`, `status`, `created_at`
- `aftercare_projects`: `id`, `project_ref`, `investor_id`, `project_title`, `current_stage`, `assigned_aftercare_officer`, `last_progress_report_date`
- `aftercare_progress_reports`: `id`, `project_id`, `phase_name`, `single_pdf_document_name`, `single_pdf_file_url`, `executive_summary`, `status`, `staff_comments`, `internal_staff_notes`
- `payments`: `id`, `invoice_ref`, `application_ref`, `service_title`, `amount_ngn`, `status`, `proof_receipt_url`
- `notifications`: `id`, `user_id`, `title`, `message`, `category`, `is_read`, `created_at`, `link_url`
- `audit_logs`: `id`, `timestamp`, `actor_id`, `actor_name`, `actor_role`, `action`, `resource_type`, `resource_ref`, `details`

#### 2. Expected Storage Buckets
- `application-dossiers` (Private, RLS restricted to applicant & assigned staff)
- `aftercare-reports` (Private, Single-PDF progress reports only)
- `payment-proofs` (Private, KGIRS & applicant only)
- `statutory-handbooks` (Public read-only)

---

## 🔐 Environment Variables (`.env.example`)

See `.env.example` in the project root:
```env
VITE_APP_NAME=KIOSC
VITE_APP_ENV=development
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```
*Note: Never include Supabase Service-Role private keys in client-side code.*

---

## 📋 Status Checklist: Production Readiness & Government Confirmations

| Module / Feature | Current State | Notes |
| :--- | :--- | :--- |
| Public Website Pages | Complete | 11 responsive, accessible public routes with interactive guide reader. |
| 10-Step Application Wizard | Complete | Full validation, 16 checklist categories, document upload progress, and draft saving. |
| Role Switcher Simulator | Active in Dev | Floating toolbar with instant 1-click switching across all 6 roles and live Lagos clock. |
| Investor Portal | Complete | Dashboard, applications, messages, print receipt modal, GRM, Aftercare. |
| Agency Workspace | Complete | Unified dashboard, triage queue, desk allocation, SLA tracker, case workbench. |
| MDA Workspace | Complete | Scoped cases, technical checklist, findings, report upload, concurrence. |
| Payment Operations UI | Complete / Default Disabled | Disabled pending official State Executive Council gazette and gateway integration. |
| Grievance Redress (GRM) | Complete | Category-based dispute lodging, independent tracking, applicant/internal notes. |
| Aftercare Management | Complete | Enforces the mandatory Single-PDF progress reporting rule per phase. |
| Platform / Superadmin | Complete | User roles, dynamic form builder, workflow stages, anti-lockout protection. |
| Supabase Backend & RLS | **Pending Phase 2** | Ready for Antigravity backend connection and table provisioning. |
| Statutory Fee Schedules | Awaiting Confirmation | Marked as *“To be confirmed by KIOSC administrator / Official State Gazette”*. |
