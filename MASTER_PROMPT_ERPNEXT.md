# Master Prompt: Generate Complete ERPNext-Based MISA AMIS Implementation

## Instructions for AI Assistant

You are tasked with generating **complete, production-ready documentation** for building a MISA AMIS clone ERP system on top of ERPNext v17.

### Project Context

- **Base Platform:** ERPNext v17 at `/Users/thanhson/Workspace/abn.erp.erpnext`
- **HR System:** Frappe HRMS (separate app, required for HR/Payroll)
- **Reference:** 41 MISA AMIS module specifications at `/Users/thanhson/Workspace/abn.erp.misa/docs/M01-M41_*.md`
- **Screenshots:** 73 MISA AMIS screenshots at `/Users/thanhson/Workspace/abn.erp.misa/assets/image_*.png`
- **Goal:** Build complete MISA AMIS clone using ERPNext as foundation + custom Frappe apps

### Custom Apps to Build

| App | Purpose | Modules Covered |
|-----|---------|----------------|
| `misa_vietnam` | Vietnam localization | M01-M11 (partial), M33, M34, M36, M37, M41 |
| `misa_hr_extended` | HR beyond HRMS | M12-M18 |
| `misa_digital_office` | Collaboration tools | M19-M30 |
| `misa_esign` | Electronic signatures | M29, M35 |
| `misa_oneai` | AI platform | M38 |
| `misa_marketing` | Marketing automation | M31 |
| `misa_operations` | Operations dashboard | M39, M40 |

---

## Execution Sequence

Generate documents in this EXACT order. Each document must be complete before moving to the next.

### Phase 1: Foundation (Generate D01-D04)

**Prompt 1: Architecture Document**
```
Read: /Users/thanhson/Workspace/abn.erp.misa/IMPLEMENTATION_PLAN.md
Read: /Users/thanhson/Workspace/abn.erp.erpnext/erpnext/modules.txt
Read: All 41 module specs from /Users/thanhson/Workspace/abn.erp.misa/docs/

Generate: ARCHITECTURE.md

Include:
1. System overview diagram (ASCII)
2. App dependency graph (ASCII)
3. Data flow between apps
4. Technology stack details
5. Deployment architecture
6. Security model
7. Performance architecture
8. Integration points overview
9. Directory structure for all custom apps
10. Naming conventions

Minimum 30 pages. Be specific to this project, not generic.
```

**Prompt 2: Development Guide**
```
Read: ARCHITECTURE.md (just generated)
Read: /Users/thanhson/Workspace/abn.erp.erpnext/erpnext/hooks.py

Generate: DEVELOPMENT.md

Include:
1. Development environment setup (step-by-step)
2. Git workflow (branching strategy, PR process)
3. Coding standards (Python, JavaScript, Jinja)
4. Doctype creation guide with examples
5. API development guide with examples
6. Testing strategy (unit, integration, E2E)
7. Code review checklist
8. Common Frappe patterns and anti-patterns
9. Debugging guide
10. Performance best practices

Include actual code examples for each section.
```

**Prompt 3: Gap Analysis**
```
Read: All 41 module specs from /Users/thanhson/Workspace/abn.erp.misa/docs/
Read: /Users/thanhson/Workspace/abn.erp.erpnext/erpnext/modules.txt
Read: ERPNext doctype list (from previous analysis: ~537 doctypes)

Generate: GAP_ANALYSIS.md

For EACH of the 41 MISA modules (M01-M41):
1. MISA feature description
2. ERPNext equivalent (doctype/module)
3. Coverage percentage
4. Gap description
5. Recommended approach (extend/use/build)
6. Effort estimate
7. Priority

Include:
- Summary table of all 41 modules
- Overall coverage statistics
- Risk assessment
- Recommendations

Be specific with doctype names and module references.
```

**Prompt 4: Module Mapping**
```
Read: GAP_ANALYSIS.md (just generated)
Read: All 41 module specs
Read: IMPLEMENTATION_PLAN.md

Generate: MODULE_MAPPING.md

For EACH custom app (misa_vietnam, misa_hr_extended, etc.):
1. App purpose and scope
2. MISA modules it covers
3. ERPNext doctypes it extends
4. New doctypes it introduces
5. API endpoints it provides
6. Dependencies on other apps
7. Configuration requirements
8. Installation steps

Include:
- Complete app catalog
- Dependency graph between apps
- Installation order
- Configuration matrix
```

---

### Phase 2: Technical Specifications (Generate D05-D08)

**Prompt 5: API Specifications**
```
Read: All 41 module specs
Read: GAP_ANALYSIS.md
Read: MODULE_MAPPING.md

Generate: API_SPECIFICATIONS.md

For EACH custom app, document ALL API endpoints:
1. Endpoint path and method
2. Purpose
3. Request parameters
4. Request body schema
5. Response schema
6. Error responses
7. Permissions required
8. Rate limits
9. Example curl commands
10. Example JavaScript/Python usage

Group by app, then by resource. Include OpenAPI/Swagger definition.
```

**Prompt 6: Database Design**
```
Read: All 41 module specs
Read: ERPNext doctype list
Read: MODULE_MAPPING.md

Generate: DATABASE_DESIGN.md

For EACH custom doctype in each app:
1. Doctype name and purpose
2. Complete field definition (name, type, options, constraints)
3. Relationships to other doctypes (ERPNext and custom)
4. Indexes required
5. Naming series
6. Permissions
7. Business logic (server scripts, hooks)
8. Sample data

Include:
- ERD diagram (ASCII or Mermaid)
- Field naming conventions
- Migration strategy from MISA
- Data retention policies
```

**Prompt 7: Vietnam Compliance**
```
Read: MISA screenshots from /Users/thanhson/Workspace/abn.erp.misa/assets/
Read: Module specs M04, M12, M17, M33
Read: IMPLEMENTATION_PLAN.md Vietnam sections

Generate: VIETNAM_COMPLIANCE.md

Cover:
1. Vietnam Chart of Accounts (per Ministry of Finance)
2. VAT rates (0%, 5%, 8%, 10%) and application rules
3. E-invoice requirements (General Dept of Taxation)
4. Corporate Income Tax calculation
5. Personal Income Tax calculation
6. Social Insurance contributions
7. Health Insurance contributions
8. Labor law compliance (overtime, leave, minimum wage)
9. Vietnamese address format
10. Vietnamese number formatting

Include:
- Legal references
- Calculation formulas
- Report layouts
- Print format specifications
- Validation rules
```

**Prompt 8: Integration Guide**
```
Read: Module specs M33, M34, M35, M38, M41
Read: API_SPECIFICATIONS.md

Generate: INTEGRATION_GUIDE.md

For EACH integration:
1. Vietnam E-Invoice API (GDT)
2. Payment Gateways (MOMO, ZaloPay, VNPay, Viettel Money)
3. AI Models (OpenAI, Gemini, Grok, DeepSeek, Claude)
4. Vietnamese Banks (for reconciliation)
5. SMS Provider (Vietnam)
6. Zalo OA API
7. Facebook Graph API

Include for each:
- API documentation link
- Authentication method
- Rate limits
- Webhook configuration
- Error handling
- Retry strategy
- Testing sandbox details
```

---

### Phase 3: Module Specifications (Generate D09-D15)

**Prompt 9: MISA Vietnam Spec**
```
Read: Module specs M01-M11, M33, M34, M36, M37, M41
Read: GAP_ANALYSIS.md
Read: VIETNAM_COMPLIANCE.md

Generate: MISA_VIETNAM_SPEC.md

Document complete specification for misa_vietnam app:
1. App overview
2. All custom doctypes with full definitions
3. All ERPNext doctype extensions
4. All custom API endpoints
5. All hooks.py configurations
6. All client scripts
7. All print formats
8. All reports
9. All workspace configurations
10. Installation and configuration guide

Be production-ready quality. Include code examples.
```

**Prompt 10: MISA HR Extended Spec**
```
Read: Module specs M12-M18
Read: GAP_ANALYSIS.md
Read: HRMS app structure (if available)

Generate: MISA_HR_EXTENDED_SPEC.md

Document complete specification for misa_hr_extended app.
Follow same structure as Prompt 9.
```

**Prompt 11: MISA Digital Office Spec**
```
Read: Module specs M19-M30
Read: GAP_ANALYSIS.md

Generate: MISA_DIGITAL_OFFICE_SPEC.md

Document complete specification for misa_digital_office app.
Follow same structure as Prompt 9.
```

**Prompt 12-15: Remaining Module Specs**
```
Repeat for:
- MISA_ONEAI_SPEC.md (M38)
- MISA_MARKETING_SPEC.md (M31)
- MISA_ESIGN_SPEC.md (M29, M35)
- MISA_OPERATIONS_SPEC.md (M39, M40)

Each follows same structure as Prompt 9.
```

---

### Phase 4: Operations Documentation (Generate D16-D20)

**Prompt 16: Deployment Guide**
```
Read: ARCHITECTURE.md
Read: All module specs

Generate: DEPLOYMENT.md

Include:
1. Production server requirements
2. Docker deployment
3. Kubernetes deployment (optional)
4. Database setup and backup
5. Redis configuration
6. Nginx configuration
7. SSL/TLS setup
8. Monitoring setup (Prometheus, Grafana)
9. Log management
10. Scaling strategy
```

**Prompt 17-20: Testing, Performance, Security, Backup**
```
Generate sequentially:
- TESTING.md
- PERFORMANCE.md
- SECURITY.md
- BACKUP_RECOVERY.md

Each minimum 20 pages with specific examples.
```

---

### Phase 5: User Documentation (Generate D21-D24)

**Prompt 21: User Guide**
```
Read: All 73 MISA screenshots
Read: All module specs

Generate: USER_GUIDE.md

For EACH MISA module visible in screenshots:
1. How to access the module
2. How to use each screen
3. Common workflows
4. Tips and tricks
5. Troubleshooting

Include screenshot references from /Users/thanhson/Workspace/abn.erp.misa/assets/
```

**Prompt 22-24: Admin Guide, Quick Start, FAQ**
```
Generate sequentially:
- ADMIN_GUIDE.md
- QUICK_START.md
- FAQ.md
```

---

### Phase 6: Migration Documentation (Generate D25-D27)

**Prompt 25: Migration Plan**
```
Read: All module specs
Read: GAP_ANALYSIS.md
Read: DATABASE_DESIGN.md

Generate: MIGRATION_PLAN.md

Include:
1. Migration strategy
2. Data mapping approach
3. Migration tools
4. Testing migration
5. Rollback plan
6. Timeline
```

**Prompt 26-27: Data Mapping, Cutover**
```
Generate:
- DATA_MAPPING.md (field-by-field MISA → ERPNext)
- CUTOVER_PLAN.md (go-live strategy)
```

---

## Output Requirements

- Save each document to `/Users/thanhson/Workspace/abn.erp.misa/docs/{document_name}.md`
- Each document must be complete before moving to next
- Reference actual file paths, doctype names, module numbers
- Include code examples, not just descriptions
- Minimum page counts as specified
- All documents must be internally consistent

## Quality Gates

Before marking a document complete:
1. All sections filled (no TODOs)
2. Code examples are syntactically correct
3. References to other documents are accurate
4. ERNext doctype names are correct
5. MISA module references are accurate
6. Vietnamese terms are correctly spelled
7. Formatting is consistent with other documents

Begin with Prompt 1 (Architecture Document) now.
