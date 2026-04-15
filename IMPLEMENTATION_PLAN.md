# MISA AMIS on ERPNext - Complete Implementation Plan & Gap Analysis

**Target:** Build MISA AMIS clone ERP on top of ERPNext v17  
**Base Platform:** ERPNext 17.0.0-dev (Frappe Framework)  
**Reference:** 73 MISA AMIS screenshots + 41 module specifications  
**Created:** April 14, 2026  

---

## 📊 Executive Summary

### Current State

| System | Modules | Doctypes | Reports | Notes |
|--------|---------|----------|---------|-------|
| **MISA AMIS** | 41 apps | ~400+ entities | ~150+ | Vietnamese-focused, AI-integrated |
| **ERPNext v17** | 20 modules | ~537 doctypes | ~190+ | Global ERP, no Vietnam localization |

### Key Finding
ERPNext already covers **~60% of MISA AMIS functionality** out-of-the-box. The remaining **40%** requires custom development as Frappe apps layered on top.

---

## 🗺️ Complete Feature Mapping: MISA AMIS → ERPNext

### P0 - Foundation (3 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M01 User & Authentication** | Frappe `User`, `UserProfile`, ERPNext `Employee` | 80% | Extend Employee with MISA-specific fields (ethnicity, religion, family type, etc.) |
| **M02 Roles & Permissions** | Frappe `Role`, `Has Role`, `Role Permission` | 95% | Already complete; add department-level RBAC |
| **M03 Company & Organization** | ERPNext `Company`, `Department`, `Branch` | 85% | Add subsidiary hierarchy, data consolidation, group management |

### P1 - Core (3 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M04 Accounting** | ERPNext Accounts (187 doctypes, 57 reports) | 90% | Vietnam tax compliance, e-invoice, cooperative accounting (HTX) |
| **M05 Employee Management** | ERPNext `Employee` + **HRMS App Required** | 70% | HRMS provides most; add Vietnam-specific fields, timesheet integration |
| **M06 Task Management** | ERPNext `Project` + `Task` (16 doctypes) | 75% | Add chat integration, Kanban board, personal projects view |

### P2 - Operations (5 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M07 Purchasing** | ERPNext Buying (21 doctypes) + Subcontracting | 85% | Add multi-supplier quotation comparison, approval timeline visualization |
| **M08 Sales** | ERPNext Selling (19 doctypes) + POS | 80% | POS needs enhancement for Vietnamese retail; add product image grid |
| **M09 Inventory** | ERPNext Stock (78 doctypes, 52 reports) | 95% | Already comprehensive; add Vietnamese warehouse keeper role |
| **M10 Asset Management** | ERPNext Assets (27 doctypes) | 70% | Add 11-status lifecycle, repair scheduling, reminder system |
| **M11 Contract Management** | CRM `Contract` (4 doctypes) | 60% | Extend with payment tracking, auto-renewal alerts, multi-party support |

### P3 - HR Suite (7 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M12 Payroll** | **HRMS App** (separate) | 80% | HRMS has payroll; add Vietnam PIT, social insurance, bonus schemes |
| **M13 Time Tracking** | **HRMS App** Shift + Attendance | 75% | Add Vietnamese shift codes, overtime calculation per labor law |
| **M14 Recruitment** | **HRMS App** (limited) | 50% | HRMS has basic recruitment; need full Kanban pipeline, talent pools |
| **M15 Performance Evaluation** | **HRMS App** (limited) | 40% | Build from scratch: 360° evaluation, radar charts, template library |
| **M16 Objectives & KPI** | **HRMS App** or custom | 20% | Build from scratch: MBO/OKR/KPI/BSC, mind maps, strategy maps |
| **M17 Social Insurance** | **Custom App Required** | 0% | Vietnam-specific: procedure codes 600-series, electronic declarations |
| **M18 Labor Relations** | **HRMS App** (partial) | 50% | Extend HRMS: contract lifecycle, appointments, dismissals, transfers |

### P4 - Digital Office (12 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M19 Workflow** | Frappe Workflow (built-in) | 80% | Already exists; add visual designer, department statistics |
| **M20 Internal Chat** | Frappe Chat (limited) | 30% | Build real-time chat: groups, @mentions, file sharing, task integration |
| **M21 Document Management** | Frappe File Manager + **Custom** | 50% | Build hierarchical folders, version control, ISO document management |
| **M22 Correspondence** | **Custom App Required** | 0% | Build: incoming/outgoing tracking, signatory authority |
| **M23 Social Network** | **Custom App Required** | 0% | Build: social feed, likes/comments/shares, birthday notifications |
| **M24 Meeting Room** | **Custom App Required** | 0% | Build: room booking calendar, conflict detection |
| **M25 Contact Directory** | ERPNext `Contact` | 85% | Already exists; add groups, tags, favorites |
| **M26 Video Library** | ERPNext `Video` | 70% | Already exists; add playlists, view tracking, comments |
| **M27 Photo Library** | **Custom App Required** | 0% | Build: photo albums, social engagement, full-screen viewer |
| **M28 Notes & Knowledge** | ERPNext `Wiki` or **Custom** | 40% | Build knowledge base: articles, categories, ratings, multi-author |
| **M29 Electronic Signature** | **Custom App Required** | 0% | Build: WeSign clone, multi-signer workflow, certificate management |
| **M30 URL Shortener** | **Custom App Required** | 0% | Build: URL shortening, QR codes, click analytics |

### P5 - Advanced (7 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M31 aiMarketing** | ERPNext CRM `Campaign` + `Email Campaign` | 30% | Build: landing page builder, lead forms, multi-channel (SMS/Zalo/Facebook) |
| **M32 CRM** | ERPNext CRM (29 doctypes, 10 reports) | 85% | Already comprehensive; add pipeline visualization, workflow integrations |
| **M33 E-Invoice** | **Vietnam Regional App** | 0% | Build Vietnam e-invoice per General Dept of Taxation (QR codes, XML) |
| **M34 Payment Gateway** | ERPNext `Payment Gateway` | 50% | Integrate Vietnamese gateways: MOMO, ZaloPay, Viettel Money, VNPay |
| **M35 Digital Signature** | **Custom App Required** | 0% | Certificate management: validity tracking, renewal, revocation |
| **M36 Promotion** | ERPNext `Pricing Rule` + `Promotional Scheme` | 70% | Extend: promotion effectiveness analysis, budget tracking |
| **M37 Store/POS** | ERPNext POS (15+ doctypes) | 75% | Enhance: product image grid, category filters, keyboard shortcuts |

### P6 - Platform (4 modules)

| MISA Module | ERPNext Equivalent | Coverage | Action |
|-------------|-------------------|----------|--------|
| **M38 OneAI** | **Custom App Required** | 0% | Build: multi-model AI (ChatGPT/Gemini/Grok), quota management, usage tracking |
| **M39 Operations Dashboard** | Frappe Workspace + **Custom** | 50% | Build cross-module dashboards, usage analytics, generated data summary |
| **M40 Group Management** | ERPNext multi-company | 60% | Extend: parent-subsidiary hierarchy, data consolidation, inter-company |
| **M41 E-Banking** | ERPNext `Bank Transaction` + **Integration** | 50% | Integrate Vietnamese banks: auto-reconciliation, MT940/BAI2 import |

---

## 📈 Coverage Summary

| Category | Total Modules | Fully Covered | Partially Covered | Custom Required |
|----------|--------------|---------------|-------------------|-----------------|
| P0 Foundation | 3 | 1 | 2 | 0 |
| P1 Core | 3 | 0 | 3 | 0 |
| P2 Operations | 5 | 0 | 5 | 0 |
| P3 HR Suite | 7 | 0 | 4 | 3 |
| P4 Digital Office | 12 | 0 | 4 | 8 |
| P5 Advanced | 7 | 0 | 5 | 2 |
| P6 Platform | 4 | 0 | 3 | 1 |
| **TOTAL** | **41** | **1** | **26** | **14** |

**Overall Coverage:**
- ✅ **Ready to use (80%+):** 1 module (2.4%)
- 🔧 **Needs extension (40-79%):** 26 modules (63.4%)
- 🆕 **Build from scratch (0-39%):** 14 modules (34.1%)

---

## 🏗️ Implementation Architecture

### Proposed App Structure

```
abn.erp.misa/
├── erpnext (base)                    # Untouched ERPNext v17
├── hrms (dependency)                 # Frappe HRMS app for HR/Payroll
├── misa_vietnam (custom app)         # Vietnam localization
│   ├── vietnam_tax.py                # VAT compliance, e-invoice
│   ├── chart_of_accounts/            # Vietnam COA template
│   ├── print_formats/                # Vietnamese invoices
│   └── reports/                      # VAT, CIT, PIT reports
├── misa_hr_extended (custom app)     # HR extensions beyond HRMS
│   ├── performance_evaluation/       # 360° evaluation, radar charts
│   ├── objectives_kpi/               # MBO/OKR/KPI/BSC
│   ├── social_insurance/             # Vietnam SI declarations
│   └── labor_relations/              # Contract lifecycle
├── misa_digital_office (custom app)  # Collaboration tools
│   ├── chat/                         # Real-time messaging
│   ├── social_network/               # Internal social feed
│   ├── document_management/          # Hierarchical docs
│   ├── meeting_room/                 # Room booking
│   ├── photo_library/                # Photo sharing
│   ├── correspondence/               # Incoming/outgoing tracking
│   └── url_shortener/                # ShortLink service
├── misa_esign (custom app)           # Electronic signatures
│   ├── signature_workflow/           # Multi-signer workflow
│   └── certificate_management/       # Digital certificates
├── misa_oneai (custom app)           # AI platform
│   ├── ai_models/                    # ChatGPT/Gemini/Grok integration
│   ├── quota_management/             # Usage tracking
│   └── custom_bots/                  # Bot builder
├── misa_marketing (custom app)       # Marketing automation
│   ├── landing_pages/                # No-code builder
│   ├── lead_forms/                   # Form builder
│   └── campaigns/                    # Multi-channel campaigns
└── misa_operations (custom app)      # Operations dashboard
    ├── cross_module_dashboard/       # Unified dashboard
    ├── usage_analytics/              # App usage tracking
    └── group_management/             # Parent-subsidiary
```

### Dependency Graph

```
                    ┌─────────────┐
                    │   Frappe    │
                    │  Framework  │
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │   ERPNext   │
                    │    v17      │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
       ┌──────▼──────┐ ┌───▼────┐ ┌────▼─────┐
       │    HRMS     │ │ misa_  │ │ misa_    │
       │   (Frappe)  │ │vietnam│ │operations│
       └──────┬──────┘ └───┬────┘ └────┬─────┘
              │            │            │
       ┌──────▼──────┐ ───▼────────────▼────┐
       │ misa_hr_    │ │ misa_digital_office │
       │ extended    │ └───┬────────────┬────
       └──────┬──────┘     │            │
              │       ┌─────▼─────┐ ┌────▼──────┐
       ┌──────▼──────┐ │ misa_   │ │ misa_     │
       │ misa_esign  │ │ oneai   │ │ marketing │
       └─────────────┘ └─────────┘ └───────────┘
```

---

## 📋 Phase-by-Phase Implementation Plan

### Phase 1: Foundation & Vietnam Localization (Weeks 1-4)

**Goal:** Establish base ERPNext with Vietnam compliance

| Task | Module | Effort | Dependencies |
|------|--------|--------|-------------|
| Install ERPNext v17 + HRMS | Setup | 2 days | - |
| Vietnam Chart of Accounts | misa_vietnam | 3 days | ERPNext |
| Vietnam tax templates (0%, 5%, 8%, 10%) | misa_vietnam | 2 days | COA |
| Vietnamese address template | misa_vietnam | 1 day | Setup |
| Employee form extension (ethnicity, religion, etc.) | misa_vietnam | 3 days | HRMS |
| Department-level RBAC | misa_vietnam | 3 days | Frappe |
| Vietnamese print formats (invoice, DN, PO) | misa_vietnam | 5 days | Tax |
| Vietnam VAT report | misa_vietnam | 4 days | Tax |
| Multi-company subsidiary hierarchy | misa_operations | 5 days | ERPNext |

**Deliverables:** Working ERPNext with Vietnam localization, HRMS installed, basic compliance

---

### Phase 2: HR Extensions (Weeks 5-10)

**Goal:** Complete HR suite matching MISA AMIS

| Task | Module | Effort | Dependencies |
|------|--------|--------|-------------|
| Timesheet extension (Vietnamese shifts) | misa_hr_extended | 5 days | HRMS |
| Overtime calculation per labor law | misa_hr_extended | 4 days | Timesheet |
| Payroll: Vietnam PIT, SI, HI, UI | misa_hr_extended | 7 days | HRMS Payroll |
| Performance Evaluation (360°, radar charts) | misa_hr_extended | 8 days | Employee |
| Objectives & KPI (MBO/OKR/KPI/BSC) | misa_hr_extended | 10 days | Evaluation |
| Social Insurance (600-series procedures) | misa_hr_extended | 7 days | Payroll |
| Labor Relations (contract lifecycle) | misa_hr_extended | 5 days | Employee |
| Recruitment Kanban pipeline | misa_hr_extended | 5 days | HRMS |

**Deliverables:** Complete HR suite matching MISA images 20-21, 35-54

---

### Phase 3: Operations Enhancement (Weeks 11-16)

**Goal:** Extend ERPNext operations to match MISA

| Task | Module | Effort | Dependencies |
|------|--------|--------|-------------|
| Multi-supplier quotation comparison | misa_vietnam | 5 days | Buying |
| Approval timeline visualization | misa_vietnam | 4 days | Workflow |
| POS enhancement (image grid, categories) | misa_vietnam | 5 days | POS |
| Asset lifecycle (11 statuses) | misa_vietnam | 5 days | Assets |
| Asset repair scheduling | misa_vietnam | 4 days | Asset |
| Contract management extension | misa_vietnam | 4 days | CRM |
| Task management (Kanban, chat integration) | misa_digital_office | 5 days | Projects |
| Document management (hierarchical) | misa_digital_office | 5 days | Frappe |

**Deliverables:** Operations matching MISA images 08-17, 25-34, 68-70

---

### Phase 4: Digital Office (Weeks 17-24)

**Goal:** Build collaboration tools

| Task | Module | Effort | Dependencies |
|------|--------|--------|-------------|
| Real-time Chat (groups, @mentions) | misa_digital_office | 8 days | - |
| Social Network (feed, likes, shares) | misa_digital_office | 7 days | Chat |
| Meeting Room Booking | misa_digital_office | 5 days | - |
| Photo Library (albums, viewer) | misa_digital_office | 5 days | - |
| Correspondence tracking | misa_digital_office | 4 days | - |
| URL Shortener + QR codes | misa_digital_office | 3 days | - |
| Knowledge base | misa_digital_office | 4 days | Doc Mgmt |
| Video library enhancement | misa_digital_office | 2 days | ERPNext |

**Deliverables:** Digital office matching MISA images 18-24, 55-67

---

### Phase 5: Advanced Modules (Weeks 25-32)

**Goal:** Build marketing, signatures, AI

| Task | Module | Effort | Dependencies |
|------|--------|--------|-------------|
| E-Invoice (Vietnam General Dept of Taxation) | misa_vietnam | 10 days | Tax |
| Payment Gateway (MOMO, ZaloPay, VNPay) | misa_vietnam | 7 days | Accounts |
| aiMarketing (landing pages, lead forms) | misa_marketing | 10 days | CRM |
| Multi-channel campaigns (SMS/Zalo/Facebook) | misa_marketing | 7 days | aiMarketing |
| Electronic Signature (WeSign clone) | misa_esign | 8 days | - |
| Digital Certificate management | misa_esign | 4 days | ESign |
| OneAI Platform (ChatGPT/Gemini/Grok) | misa_oneai | 10 days | - |
| Quota management + usage tracking | misa_oneai | 5 days | OneAI |

**Deliverables:** Advanced modules matching MISA images 09-10, 26-28, 57, 65-66

---

### Phase 6: Platform & Polish (Weeks 33-36)

**Goal:** Operations dashboard, group management, testing

| Task | Module | Effort | Dependencies |
|------|--------|--------|-------------|
| Cross-module operations dashboard | misa_operations | 7 days | All |
| Usage analytics per app | misa_operations | 5 days | Dashboard |
| E-Banking integration (Vietnamese banks) | misa_vietnam | 5 days | Accounts |
| Mobile app responsive design | All apps | 10 days | All |
| Performance optimization | All apps | 5 days | - |
| Security audit | All apps | 5 days | - |
| User acceptance testing | All apps | 10 days | - |
| Documentation | All apps | 5 days | - |

**Deliverables:** Production-ready system matching all 73 MISA AMIS screenshots

---

## 📊 Resource Estimation

### Development Effort

| Phase | Duration | Developer Weeks | Key Roles |
|-------|----------|----------------|-----------|
| Phase 1: Foundation | 4 weeks | 6 | Frappe dev, Vietnam tax expert |
| Phase 2: HR Extensions | 6 weeks | 12 | Frappe dev, HR specialist |
| Phase 3: Operations | 6 weeks | 8 | Frappe dev, UX designer |
| Phase 4: Digital Office | 8 weeks | 14 | Frontend dev, Real-time engineer |
| Phase 5: Advanced | 8 weeks | 14 | Full-stack, AI engineer |
| Phase 6: Platform | 4 weeks | 10 | DevOps, QA, UX |
| **TOTAL** | **36 weeks** | **64 developer-weeks** | **~4 developers full-time** |

### Team Composition

| Role | Count | Responsibilities |
|------|-------|-----------------|
| **Senior Frappe Developer** | 1 | Architecture, core modules, code review |
| **Full-Stack Developer** | 1 | Custom apps, API development, integrations |
| **Frontend Developer** | 1 | Vue.js/React components, UI/UX, responsive |
| **DevOps/QA Engineer** | 1 | Deployment, testing, CI/CD, monitoring |

**Total Timeline:** 9 months with 4-person team

---

## 🔧 Technical Approach

### Development Standards

```
Framework:     Frappe Framework v17
Base ERP:      ERPNext v17
HR System:     HRMS (Frappe)
Frontend:      Frappe UI + Vue.js components
Database:      MariaDB 10.6+
Cache:         Redis 6+
Search:        MariaDB full-text + Elasticsearch (optional)
Queue:         Redis Queue (RQ)
Real-time:     Socket.io (Frappe built-in)
AI APIs:       OpenAI, Google Gemini, Anthropic Claude, DeepSeek
Payment APIs:  MOMO, ZaloPay, VNPay, Viettel Money
E-Invoice:     Vietnam General Department of Taxation API
```

### Custom App Template

Each custom app follows this structure:

```
misa_{module}/
├── misa_{module}/
│   ├── __init__.py
│   ├── hooks.py                    # Frappe hooks
│   ├── modules.txt                 # Module registration
│   ├── patches.txt                 # Migration patches
│   ├── config/
│   │   └── desktop.py              # Workspace icons
│   ├── doctypes/                   # Custom doctypes
│   │   └── {doctype_name}/
│   │       ├── {doctype_name}.json # Doctype definition
│   │       ├── {doctype_name}.py   # Controller
│   │       ├── {doctype_name}.js   # Client script
│   │       └── test_{doctype}.py   # Tests
│   ├── api/                        # REST API endpoints
│   │   └── v1/
│   │       └── {resource}.py
│   ├── public/
│   │   ├── js/                     # Client-side JS
│   │   ├── css/                    # Styles
│   │   └── images/                 # Assets
│   ├── templates/                  # Jinja templates
│   ├── www/                        # Web pages
│   ├── workspace/                  # Frappe workspaces
│   └── fixtures/                   # Seed data
├── license.txt
├── pyproject.toml                  # Python dependencies
├── package.json                    # Node dependencies
└── README.md
```

### Integration Strategy

| Integration | Method | Notes |
|-------------|--------|-------|
| **Vietnam E-Invoice** | REST API to GDT | XML generation, QR codes, digital signature |
| **Vietnam Banks** | MT940/BAI2 import | Auto-reconciliation, multi-bank |
| **Payment Gateways** | Webhook-based | MOMO, ZaloPay, VNPay, Viettel Money |
| **AI Models** | OpenAI/Gemini/Claude APIs | Quota management, usage tracking |
| **SMS** | Vietnamese SMS provider | Twilio alternative for Vietnam |
| **Zalo** | Zalo OA API | Business messaging integration |
| **Facebook** | Facebook Graph API | Marketing campaigns |

---

## 📝 Documentation Plan

### Documents to Create

| Document | Purpose | Priority |
|----------|---------|----------|
| **ARCHITECTURE.md** | System architecture, app structure, dependency graph | P0 |
| **DEVELOPMENT.md** | Setup guide, coding standards, Git workflow | P0 |
| **API_REFERENCE.md** | All custom API endpoints with examples | P1 |
| **VIETNAM_COMPLIANCE.md** | Tax, e-invoice, labor law compliance details | P1 |
| **DEPLOYMENT.md** | Production deployment, Docker, monitoring | P1 |
| **MIGRATION.md** | Data migration from MISA AMIS if needed | P2 |
| **USER_GUIDE.md** | End-user documentation per module | P2 |
| **TESTING.md** | Test strategy, coverage requirements | P2 |
| **PERFORMANCE.md** | Performance benchmarks, optimization guide | P3 |
| **SECURITY.md** | Security audit checklist, penetration testing | P3 |

### Documentation Structure

```
docs/
├── 01-getting-started/
│   ├── installation.md
│   ├── configuration.md
│   └── quick-start.md
├── 02-architecture/
│   ├── system-overview.md
│   ├── app-structure.md
│   ├── dependency-graph.md
│   └── data-flow.md
├── 03-modules/
│   ├── misa_vietnam/
│   ├── misa_hr_extended/
│   ├── misa_digital_office/
│   ├── misa_esign/
│   ├── misa_oneai/
│   ├── misa_marketing/
│   └── misa_operations/
├── 04-integrations/
│   ├── vietnam-einvoice.md
│   ├── payment-gateways.md
│   ├── ai-models.md
│   └── banking.md
├── 05-development/
│   ├── coding-standards.md
│   ├── doctype-creation.md
│   ├── api-development.md
│   ├── testing.md
│   └── deployment.md
├── 06-compliance/
│   ├── vietnam-tax.md
│   ├── vietnam-labor-law.md
│   ├── social-insurance.md
│   └── data-protection.md
└── 07-migration/
    ├── from-misa-amis.md
    ├── data-mapping.md
    └── cutover-plan.md
```

---

## ✅ Next Steps

1. **Review this plan** and confirm scope
2. **Set up development environment** with ERPNext v17 + HRMS
3. **Create custom app skeleton** for `misa_vietnam`
4. **Implement Phase 1** (Vietnam localization)
5. **Iterate through phases** with weekly demos

### Immediate Actions

```bash
# 1. Verify ERPNext installation
cd /Users/thanhson/Workspace/abn.erp.erpnext
git status

# 2. Install HRMS app
bench get-app hrms https://github.com/frappe/hrms
bench install-app hrms

# 3. Create custom app
bench new-app misa_vietnam
bench install-app misa_vietnam

# 4. Set up Vietnam Chart of Accounts
# (To be implemented in misa_vietnam)
```

---

*Plan generated April 14, 2026 based on 73 MISA AMIS screenshots and ERPNext v17 analysis*
