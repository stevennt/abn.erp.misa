# ERP Module Documentation - Master Index & Automation Guide

**Project:** abn.erp.misa (MISA AMIS Clone)  
**Source:** GitHub Issue #632 - 73 screenshots  
**Total Modules:** 41+ applications across 5 categories  
**Generated:** April 14, 2026  

---

## 📚 Quick Start

1. Copy the **Master Prompt** from `MASTER_PROMPT.md`
2. Paste it into your AI assistant
3. It will automatically generate all 41 module specifications sequentially
4. Total expected output: ~400-500 pages of technical documentation

---

## 🗂️ Module Index by Priority

### Priority 0 - Foundation (3 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M01 | User & Authentication | 02, 35, 37 | None |
| M02 | Roles, Permissions & Access Control | 02, 35, 60 | M01 |
| M03 | Company & Organization Structure | 29, 30, 37, 39, 60 | M01, M02 |

### Priority 1 - Core (3 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M04 | Accounting (Kế toán) | 15, 25, 28, 29, 62 | M01-M03 |
| M05 | Employee Management (Nhân viên) | 21, 35, 37, 38 | M01-M03 |
| M06 | Task Management (Công việc) | 17, 22, 55, 60 | M01-M03 |

### Priority 2 - Operations (5 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M07 | Purchasing (Mua hàng) | 29, 68, 69, 70 | M01-M06 |
| M08 | Sales (Bán hàng) | 34, 62 | M01-M06 |
| M09 | Inventory & Warehouse (Kho hàng) | 01, 29, 68 | M07, M08 |
| M10 | Asset Management (Tài sản) | 08, 11, 12, 59 | M01-M06 |
| M11 | Contract Management (Hợp đồng) | 01, 62 | M01-M06 |

### Priority 3 - HR Suite (7 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M12 | Payroll (Tiền lương) | 20, 50, 51, 63 | M05, M21 |
| M13 | Time Tracking (Chấm công) | 21, 38, 47, 48, 49 | M05 |
| M14 | Recruitment (Tuyển dụng) | 36, 63 | M05 |
| M15 | Performance Evaluation (Đánh giá) | 43, 44, 45, 46 | M05, M16 |
| M16 | Objectives & KPI (Mục tiêu) | 39, 40, 41, 42 | M05, M03 |
| M17 | Social Insurance (Bảo hiểm xã hội) | 52, 53, 54 | M05, M12 |
| M18 | Labor Relations (Quan hệ lao động) | 35, 37, 63 | M05 |

### Priority 4 - Digital Office (12 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M19 | Workflow (Quy trình) | 13, 56, 61 | M01-M03 |
| M20 | Internal Chat (Chat) | 19, 22, 58 | M01 |
| M21 | Document Management (Ghi chép/Tài liệu) | 18, 57 | M01 |
| M22 | Correspondence (Văn thư) | 18, 60 | M01 |
| M23 | Internal Social Network (Mạng xã hội) | 19, 58 | M01 |
| M24 | Meeting Room Booking (Phòng họp) | 16, 60 | M01, M03 |
| M25 | Contact Directory (Danh bạ) | 03, 04, 05 | M01 |
| M26 | Video Library (Kho phim) | 23 | M01 |
| M27 | Photo Library (Kho ảnh) | 24, 72 | M01 |
| M28 | Notes & Knowledge (Kiến thức) | 18, 72 | M01 |
| M29 | Electronic Signature (WeSign) | 57, 64 | M01 |
| M30 | URL Shortener (MISA ShortLink) | 65 | M01 |

### Priority 5 - Advanced (7 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M31 | aiMarketing | 09, 10, 14, 31, 32, 60 | M05, M08 |
| M32 | CRM | 60, 67 | M05, M08 |
| M33 | E-Invoice (Hóa đơn điện tử) | 28 | M04, M08 |
| M34 | Payment Gateway (Cổng thanh toán) | 27 | M04, M08 |
| M35 | Digital Signature Service (Chữ ký số) | 26 | M01 |
| M36 | Promotion Management (Khuyến mại) | 33 | M08 |
| M37 | Store/Retail POS (Cửa hàng) | 34 | M08, M09 |

### Priority 6 - Platform (4 modules)
| # | Module | Reference Images | Depends On |
|---|--------|-----------------|------------|
| M38 | OneAI Platform | 03, 66, 72 | M01 |
| M39 | Operations Dashboard (Điều hành) | 60, 61, 62, 63 | All modules |
| M40 | Group Management (Quản lý tập đoàn) | 30, 60 | M01-M03 |
| M41 | E-Banking (Ngân hàng điện tử) | 15, 27 | M04 |

---

## 📋 Module Context Reference

For each module, the following context is pre-extracted from the 73 screenshots and available for the AI to use:

### M01 - User & Authentication
**Images:** 02, 35, 37  
**Key Screens:**
- Welcome/onboarding email (img_02)
- Employee profile detail page with fields: employee ID, name, gender, DOB, place of birth, marital status, tax ID, family type, ethnicity, religion (img_35)
- Employee list with search, filter, pagination (img_35)
- HR dashboard with total employees, new hires, probation success, resignations (img_37)
**Data Entities:** User, Employee, UserProfile, LoginSession, PasswordReset, EmailVerification  
**Key Features:** User registration, login, profile management, employee directory, onboarding workflow

### M02 - Roles, Permissions & Access Control
**Images:** 02, 35, 60  
**Key Screens:**
- Permission assignment in workflows (img_02 mentions approval workflows)
- Department-based access (img_35 shows department assignment)
- System administration panel (img_60 shows system admin dashboard)
**Data Entities:** Role, Permission, UserRole, RolePermission, AccessLog, Department  
**Key Features:** RBAC, department-level permissions, access logging, permission inheritance

### M03 - Company & Organization Structure
**Images:** 29, 30, 37, 39, 60  
**Key Screens:**
- Cooperative structure (img_29)
- Group/corporation data management with parent-subsidiary hierarchy (img_30)
- Department structure chart (img_37)
- Organizational tree for objectives (img_39)
- System admin org view (img_60)
**Data Entities:** Company, Department, Subsidiary, OrgChart, Location, Branch  
**Key Features:** Multi-company support, org hierarchy, branch management, data consolidation

### M04 - Accounting (Kế toán)
**Images:** 15, 25, 28, 29, 62  
**Key Screens:**
- Financial dashboard with cash, deposit, receivable, payable, revenue, expense, profit (img_15)
- Receivables/payables aging (img_15)
- Revenue/expense/profit monthly chart (img_15)
- Cash flow chart (img_15)
- Sales and purchasing workflow diagrams (img_25)
- E-invoice management (img_28)
- Cooperative accounting purchasing dashboard (img_29)
- Operations financial admin dashboard (img_62)
**Data Entities:** Account, JournalEntry, GeneralLedger, CashBook, DepositBook, Receivable, Payable, Invoice, TaxDeclaration, FinancialReport, Budget  
**Key Features:** Double-entry bookkeeping, chart of accounts, financial statements, cash flow management, tax reporting, budget tracking

### M05 - Employee Management (Nhân viên)
**Images:** 21, 35, 37, 38  
**Key Screens:**
- Employee list with 100+ employees, search, filter (img_35)
- Employee detail with tabs: basic info, contact, work info, account, family (img_35)
- HR dashboard with employee count, new hires, resignations (img_37)
- Department breakdown chart (img_37)
- Contract type statistics (img_37)
- Timesheet view (img_21, img_38)
**Data Entities:** Employee, EmployeeProfile, ContactInfo, WorkHistory, FamilyMember, Contract, Document  
**Key Features:** Employee CRUD, profile management, document attachment, org assignment, contract tracking

### M06 - Task Management (Công việc)
**Images:** 17, 22, 55, 60  
**Key Screens:**
- Task dashboard with today's tasks summary, weekly donut chart, task list (img_17)
- Task creation from chat integration (img_22)
- Personal projects, department task views (img_17)
- Operations dashboard showing task counts (img_60)
**Data Entities:** Task, TaskProject, TaskAssignment, TaskComment, TaskAttachment, TaskChecklist, TaskPriority, TaskStatus  
**Key Features:** Task CRUD, assignment, priority, deadline, status tracking, checklist, comments, attachments, chat integration, department views

### M07 - Purchasing (Mua hàng)
**Images:** 29, 68, 69, 70  
**Key Screens:**
- Purchasing dashboard with order values, contract values, payment status (img_29)
- Supplier debt analysis (img_29)
- Quotation approval workflow with status tracking (img_68)
- Purchase request list with purposes, statuses (img_69)
- Quotation detail with supplier comparison, approval history timeline (img_70)
**Data Entities:** PurchaseRequest, PurchaseOrder, Quotation, Supplier, SupplierQuotation, PurchaseContract, PurchaseApproval, PurchaseItem  
**Key Features:** Purchase requisition, quotation comparison, multi-supplier bidding, approval workflow, PO creation, supplier management, delivery tracking

### M08 - Sales (Bán hàng)
**Images:** 34, 62  
**Key Screens:**
- Retail POS interface with product grid, shopping cart, checkout (img_34)
- Product categories, pricing, discount application (img_34)
- Sales data in operations dashboard (img_62)
**Data Entities:** SaleOrder, SaleItem, Customer, PriceList, Discount, SaleInvoice, SaleReturn, POS_Transaction  
**Key Features:** POS operations, product catalog, customer management, pricing, discounts, invoicing, returns

### M09 - Inventory & Warehouse (Kho hàng)
**Images:** 01, 29, 68  
**Key Screens:**
- Warehouse mentioned in core modules diagram (img_01)
- Supplier debt and purchase values related to inventory (img_29)
- Warehouse connection in workflow integrations (img_68)
**Data Entities:** Warehouse, Location, StockItem, StockMovement, StockAdjustment, StockTransfer, StockCount, StockAlert  
**Key Features:** Multi-warehouse support, stock tracking, movement logging, stock counts, alerts, transfers

### M10 - Asset Management (Tài sản)
**Images:** 08, 11, 12, 59  
**Key Screens:**
- Asset product detail with video preview (img_08)
- Asset dashboard with status donut chart (165 total assets, 60 in use, 30 not used, 24 under maintenance, etc.) (img_11)
- Asset movement bar/line chart (img_11)
- Asset repair status chart (img_11)
- Reminders by timeframe (img_11)
**Data Entities:** Asset, AssetCategory, AssetStatus, AssetMovement, AssetAllocation, AssetRecovery, AssetTransfer, AssetRepair, AssetMaintenance, AssetDisposal, AssetInventory, AssetReminder  
**Key Features:** Asset lifecycle tracking, allocation/recovery, transfer, repair/maintenance, loss/destruction/disposal, inventory, reporting

### M11 - Contract Management (Hợp đồng)
**Images:** 01, 62  
**Key Screens:**
- Contracts listed in core modules (img_01)
- Financial contracts in operations dashboard (img_62)
**Data Entities:** Contract, ContractTemplate, ContractParty, ContractTerm, ContractPayment, ContractRenewal, ContractAlert  
**Key Features:** Contract creation, template management, party management, payment tracking, renewal alerts

### M12 - Payroll (Tiền lương)
**Images:** 20, 50, 51, 63  
**Key Screens:**
- Payroll dashboard with total salary, tax, insurance (img_20)
- Employee salary level analysis bar chart (img_20)
- Income structure donut chart (base salary 54.6%, sales bonus 28.6%, etc.) (img_20)
- Average income over time line chart (img_20)
- Average income by office bar chart (img_20)
- Payslip feedback and reminders (img_20)
- Income summary by position with salary components A/B/C (img_51)
**Data Entities:** PayrollPeriod, EmployeePayroll, SalaryComponent, SalaryPolicy, Payslip, PayrollAdjustment, TaxDeclaration, InsuranceDeclaration, PayrollReport  
**Key Features:** Salary calculation, component management, policy configuration, payslip generation, tax withholding, insurance deduction, reporting

### M13 - Time Tracking (Chấm công)
**Images:** 21, 38, 47, 48, 49  
**Key Screens:**
- Monthly timesheet calendar with shift blocks (CA SÁNG, CA CHIỀU) (img_21)
- Overtime tracking popup (img_21)
- Late/early leave indicators (img_21)
- Time tracking overview dashboard with late/early counts, absence stats (img_47)
- Leave type analysis donut chart (img_47)
- Most late/early employees bar chart (img_47)
- Shift schedule setup form with calendar preview (img_48)
- Comprehensive shift schedule table with shift selection modal (img_49)
**Data Entities:** Timesheet, Shift, ShiftSchedule, AttendanceRecord, OvertimeRecord, LeaveRequest, LeaveType, LateEarlyRecord, BusinessTrip  
**Key Features:** Shift scheduling, attendance tracking, overtime calculation, leave management, late/early monitoring, business trip tracking

### M14 - Recruitment (Tuyển dụng)
**Images:** 36, 63  
**Key Screens:**
- Candidate pipeline Kanban board with stages: Applied (2), Skills Test (10), IQ Test (3), Interview (5), Hired (0), Recruited (1) (img_36)
- Job posting detail with headcount (img_36)
- Resignation reasons in operations dashboard (img_63)
**Data Entities:** JobPosting, Candidate, Application, Interview, TestResult, OfferLetter, RecruitmentStage, TalentPool  
**Key Features:** Job posting, candidate sourcing, pipeline management, interview scheduling, assessment, offer management, talent pool

### M15 - Performance Evaluation (Đánh giá)
**Images:** 43, 44, 45, 46  
**Key Screens:**
- Create evaluation period form with methods (objectives, competency, work report) (img_43)
- Employee selection table for evaluation (img_43)
- Evaluation Gantt chart timeline view (img_44)
- Individual evaluation results with radar/spider chart (img_45)
- Evaluation template library by department (img_46)
**Data Entities:** EvaluationPeriod, EvaluationMethod, EvaluationForm, EvaluationCriteria, EmployeeEvaluation, SelfEvaluation, ManagerEvaluation, EvaluationResult, EvaluationTemplate  
**Key Features:** Evaluation period creation, multi-method evaluation, competency assessment, radar chart visualization, template library, 360-degree feedback

### M16 - Objectives & KPI (Mục tiêu)
**Images:** 39, 40, 41, 42  
**Key Screens:**
- Objectives table with MBO/OKR/KPI types, progress bars (img_39)
- Mind map view of hierarchical objectives (img_40)
- Balanced Scorecard strategy map with 4 perspectives (img_41)
- Add objective form with type selection, value settings (img_42)
**Data Entities:** Objective, ObjectiveType, ObjectiveValue, ObjectiveAssignment, ObjectiveProgress, BSC_Perspective, StrategicMap, KR (Key Result)  
**Key Features:** MBO, OKR, KPI, BSC support, hierarchical objectives, progress tracking, strategy mapping, quarterly breakdowns

### M17 - Social Insurance (Bảo hiểm xã hội)
**Images:** 52, 53, 54  
**Key Screens:**
- Electronic records list with statuses (not sent, sent, missing info, rejected, approved) (img_52)
- Create electronic record modal with procedure codes (600, 600c, 600d, 601, etc.) (img_53)
- Employee list for SI submission with SI numbers, ID, DOB, department (img_54)
**Data Entities:** SIRecord, SIProcedure, SIType, EmployeeSI, SIContribution, SIDeclaration, SIReport  
**Key Features:** Electronic SI declarations, procedure code management, employee SI tracking, contribution calculation, submission status tracking

### M18 - Labor Relations (Quan hệ lao động)
**Images:** 35, 37, 63  
**Key Screens:**
- Employee database with labor relations context (img_35)
- HR overview with contracts, appointments, dismissals, transfers, resignations (img_37)
- Resignation analysis in operations dashboard (img_63)
**Data Entities:** LaborContract, Appointment, Dismissal, Transfer, Resignation, Commendation, Incident, LaborDispute  
**Key Features:** Contract management, personnel actions tracking, dispute management, commendation recording

### M19 - Workflow (Quy trình)
**Images:** 13, 56, 61  
**Key Screens:**
- Workflow run history list with ID, title, creator, date, status, executor, deadline (img_13)
- Workflow runs with custom filters (img_56)
- Operations dashboard showing workflow statistics by department (img_61)
**Data Entities:** Workflow, WorkflowStep, WorkflowRun, WorkflowAction, WorkflowApproval, WorkflowTemplate, WorkflowFilter  
**Key Features:** Workflow design, run execution, approval chains, custom filters, department statistics, status tracking

### M20 - Internal Chat (Chat)
**Images:** 19, 22, 58  
**Key Screens:**
- Chat integration with task creation (img_22)
- Group chat with file sharing, mentions (img_22)
- Social network feed with sharing features (img_19, img_58)
**Data Entities:** ChatRoom, ChatMessage, ChatMember, ChatMention, ChatFile, ChatReaction  
**Key Features:** Group chat, direct messages, file sharing, @mentions, reactions, task creation from chat

### M21 - Document Management (Ghi chép/Tài liệu)
**Images:** 18, 57  
**Key Screens:**
- Document library with hierarchical categories (img_18)
- Document list with name, description, creator, dates (img_18)
- Categories: management docs, process docs, handbooks, policies (img_18)
**Data Entities:** Document, DocumentCategory, DocumentVersion, DocumentPermission, DocumentComment, DocumentFavorite  
**Key Features:** Hierarchical document organization, version control, search, permissions, favorites, comments

### M22 - Correspondence (Văn thư)
**Images:** 18, 60  
**Key Screens:**
- Document management context (img_18)
- Operations dashboard showing correspondence stats (img_60)
**Data Entities:** Correspondence, IncomingDoc, OutgoingDoc, DocType, DocFlow, DocTracking  
**Key Features:** Incoming/outgoing document tracking, document flow management, tracking numbers

### M23 - Internal Social Network (Mạng xã hội)
**Images:** 19, 58  
**Key Screens:**
- Social feed with posts, images, likes, comments, shares (img_19)
- Post composer with share/idea/news options (img_19)
- Birthday widget, notification feed (img_19)
**Data Entities:** SocialPost, SocialComment, SocialLike, SocialShare, SocialAlbum, SocialEvent, Birthday, Announcement  
**Key Features:** Social feed, post types (share/idea/news), engagement tracking, birthday reminders, announcements

### M24 - Meeting Room Booking (Phòng họp)
**Images:** 16, 60  
**Key Screens:**
- Room booking calendar grid with building/floor navigation (img_16)
- Time slots with booked/unbooked status (img_16)
- Operations dashboard showing meeting stats (img_60)
**Data Entities:** MeetingRoom, RoomBooking, RoomSchedule, Building, Floor, RoomEquipment  
**Key Features:** Room booking calendar, building/floor navigation, time slot management, equipment tracking, recurring bookings

### M25 - Contact Directory (Danh bạ)
**Images:** 03, 04, 05  
**Key Screens:**
- Contact app icon on dashboard (img_05)
- Onboarding guide mentioning contacts (img_03, img_04)
**Data Entities:** Contact, ContactGroup, ContactTag, ContactNote  
**Key Features:** Contact management, grouping, tagging, search, notes

### M26 - Video Library (Kho phim)
**Images:** 23  
**Key Screens:**
- Video management with add video modal (link, title, thumbnail, description) (img_23)
- Video list with title, date, views (img_23)
**Data Entities:** Video, VideoCategory, VideoComment, VideoLike, VideoView  
**Key Features:** Video upload/link, categorization, view tracking, comments, likes

### M27 - Photo Library (Kho ảnh)
**Images:** 24, 72  
**Key Screens:**
- Photo viewer with full-screen display, navigation arrows (img_24)
- Photo details with uploader, date, likes, comments, shares (img_24)
- Mobile app photo library icon (img_72)
**Data Entities:** Photo, PhotoAlbum, PhotoComment, PhotoLike, PhotoShare, PhotoTag  
**Key Features:** Photo storage, album organization, social engagement (like/comment/share), tagging, full-screen viewer

### M28 - Notes & Knowledge (Kiến thức)
**Images:** 18, 72  
**Key Screens:**
- Document/knowledge management (img_18)
- Knowledge app in mobile grid (img_72)
**Data Entities:** KnowledgeArticle, KnowledgeCategory, KnowledgeTag, KnowledgeComment, KnowledgeRating  
**Key Features:** Article creation, categorization, tagging, comments, ratings, search

### M29 - Electronic Signature (WeSign)
**Images:** 57, 64  
**Key Screens:**
- WeSign dashboard with waiting for my signature (12), waiting for others (12), completed (168), rejected (4) (img_57)
- Document upload area (drag & drop, pdf/docx/doc/jpg/png) (img_57)
- Mobile app download prompt (img_57)
**Data Entities:** SignatureDocument, SignatureRequest, Signer, SignatureField, SignatureCertificate, SignatureLog  
**Key Features:** Document upload, signature request creation, multi-signer workflow, signature field placement, certificate management, audit log

### M30 - URL Shortener (MISA ShortLink)
**Images:** 65  
**Key Screens:**
- Mobile preview showing shortened URL with QR code and analytics (img_65)
**Data Entities:** ShortURL, URLAnalytics, URLClick, QRCode  
**Key Features:** URL shortening, QR code generation, click analytics, custom aliases

### M31 - aiMarketing
**Images:** 09, 10, 14, 31, 32, 60  
**Key Screens:**
- Dashboard with revenue (40.4B), cost (20.1B), ROI (28.55%) (img_09)
- Conversion funnel: Lead→MQL→SAL→SQL (img_09)
- Customer care effectiveness metrics (img_09)
- Features table: Email Marketing, Landing Page, Lead Form, Data Management, Reports, Integration (img_10)
**Data Entities:** Campaign, EmailTemplate, LandingPage, Lead, LeadForm, Contact, ContactGroup, MarketingReport, AdChannel, ConversionFunnel  
**Key Features:** Email marketing with templates, landing page builder, lead collection forms, data management, multi-channel integration, conversion tracking, real-time reports

### M32 - CRM
**Images:** 60, 67  
**Key Screens:**
- CRM in operations dashboard usage stats (img_60)
- CRM-Warehouse and CRM-Accounting workflow integrations (img_67)
**Data Entities:** Lead, Opportunity, Account, Contact, Deal, Activity, Pipeline, Stage  
**Key Features:** Lead management, opportunity tracking, account management, pipeline visualization, activity logging, workflow integrations

### M33 - E-Invoice (Hóa đơn điện tử)
**Images:** 28  
**Key Screens:**
- E-invoice dashboard with invoice statistics, timeline charts, mobile app preview (img_28)
- Tax period declaration table (img_28)
**Data Entities:** EInvoice, InvoiceItem, TaxPeriod, TaxDeclaration, InvoiceTemplate, InvoiceStatus  
**Key Features:** E-invoice creation, QR code generation, tax period management, declaration submission, status tracking, mobile access

### M34 - Payment Gateway (Cổng thanh toán)
**Images:** 27  
**Key Screens:**
- JetPay dashboard with payment stats (292.2B total, 210 transactions) (img_27)
- Refund tracking (32B, 19 transactions) (img_27)
- Net received breakdown (img_27)
- Payment value trend chart (img_27)
- Payment method pie chart (domestic card, international card, QR, MOMO, Viettel Money, Transfer, ZaloPay) (img_27)
**Data Entities:** PaymentTransaction, RefundTransaction, PaymentMethod, PaymentGateway, TransactionFee, SettlementReport  
**Key Features:** Multi-payment method support, transaction tracking, refund management, fee calculation, settlement reports, trend analytics

### M35 - Digital Signature Service (Chữ ký số)
**Images:** 26  
**Key Screens:**
- MISA ESign certificate management with certificate tree (img_26)
- Certificate info: ID, validity period, owner info (img_26)
**Data Entities:** DigitalCertificate, CertificateOwner, CertificateValidity, CertificateRevocation  
**Key Features:** Certificate management, validity tracking, owner information, revocation handling

### M36 - Promotion Management (Khuyến mại)
**Images:** 33  
**Key Screens:**
- Promotion effectiveness analysis with pie chart and data table (img_33)
- Filters: time period, date range, program, product type (img_33)
- Product purchase quantity, order count, discount amount tracking (img_33)
**Data Entities:** PromotionProgram, PromotionItem, PromotionDiscount, PromotionReport, PromotionCondition  
**Key Features:** Promotion program creation, condition setting, discount calculation, effectiveness analysis, reporting

### M37 - Store/Retail POS (Cửa hàng)
**Images:** 34  
**Key Screens:**
- POS interface with product grid, shopping cart, checkout (img_34)
- Product cards with images, prices (img_34)
- Category filters, search (img_34)
- Keyboard shortcuts (F10 save, F12 collect payment) (img_34)
**Data Entities:** POS_Session, POS_Transaction, POS_Item, POS_Customer, POS_PriceList, POS_Discount, POS_Return  
**Key Features:** POS operations, product catalog with images, shopping cart, customer lookup, price lists, discounts, returns, keyboard shortcuts

### M38 - OneAI Platform
**Images:** 03, 66, 72  
**Key Screens:**
- Onboarding guide showing OneAI assistant (img_03)
- OneAI product page with integrated models (ChatGPT, Gemini, Grok, DeepSeek, Claude, +10) (img_66)
- Benefits for organization: cost savings, centralized management, detailed reports (img_66)
- Benefits for users: productivity boost, optimized experience (img_66)
- Mobile app OneAI integration (img_72)
**Data Entities:** AIModel, AIConversation, AIPrompt, AITemplate, AIUsage, AIQuota, AICost, AIBot  
**Key Features:** Multi-model AI integration, conversation management, prompt library, template system, usage tracking, quota management, cost optimization, custom bot creation

### M39 - Operations Dashboard (Điều hành)
**Images:** 60, 61, 62, 63  
**Key Screens:**
- System admin dashboard with total users (2800), access trends, per-app usage table (img_60)
- Product usage chart and generated data summary (img_60)
- Workflow statistics by department (img_61)
- Financial management dashboard with revenue (65.3B), expenses (51.2B), profit (5.1B) (img_62)
- HR management dashboard with employee count (130), age distribution, movement trends, resignation reasons (img_63)
**Data Entities:** DashboardWidget, DashboardConfig, UsageMetric, SystemLog, AggregatedReport  
**Key Features:** Cross-module dashboards, real-time metrics, usage analytics, financial overview, HR overview, custom widget configuration

### M40 - Group Management (Quản lý tập đoàn)
**Images:** 30, 60  
**Key Screens:**
- Data management with parent-subsidiary hierarchy (img_30)
- Org structure with consolidated data view (img_30)
- System admin group management (img_60)
**Data Entities:** GroupCompany, Subsidiary, ConsolidatedData, InterCompanyTransaction, GroupReport  
**Key Features:** Multi-entity management, data consolidation, inter-company transactions, group reporting, org hierarchy

### M41 - E-Banking (Ngân hàng điện tử)
**Images:** 15, 27  
**Key Screens:**
- Banking integration in accounting dashboard (img_15)
- E-banking connection in marketplace (img_06)
- Payment gateway banking stats (img_27)
**Data Entities:** BankAccount, BankTransaction, BankStatement, BankReconciliation, BankConnection  
**Key Features:** Bank account management, transaction sync, statement import, reconciliation, multi-bank support

---

## 🔄 Automation Flow

The master prompt will execute this sequence automatically:

```
START
  ↓
[Generate M01 - User & Authentication]
  ↓
[Generate M02 - Roles & Permissions]
  ↓
[Generate M03 - Company & Organization]
  ↓
[Generate M04 - Accounting]
  ↓
[Generate M05 - Employee Management]
  ↓
[Generate M06 - Task Management]
  ↓
[Generate M07 - Purchasing]
  ↓
[Generate M08 - Sales]
  ↓
[Generate M09 - Inventory & Warehouse]
  ↓
[Generate M10 - Asset Management]
  ↓
[Generate M11 - Contract Management]
  ↓
[Generate M12 - Payroll]
  ↓
[Generate M13 - Time Tracking]
  ↓
[Generate M14 - Recruitment]
  ↓
[Generate M15 - Performance Evaluation]
  ↓
[Generate M16 - Objectives & KPI]
  ↓
[Generate M17 - Social Insurance]
  ↓
[Generate M18 - Labor Relations]
  ↓
[Generate M19 - Workflow]
  ↓
[Generate M20 - Internal Chat]
  ↓
[Generate M21 - Document Management]
  ↓
[Generate M22 - Correspondence]
  ↓
[Generate M23 - Social Network]
  ↓
[Generate M24 - Meeting Room Booking]
  ↓
[Generate M25 - Contact Directory]
  ↓
[Generate M26 - Video Library]
  ↓
[Generate M27 - Photo Library]
  ↓
[Generate M28 - Notes & Knowledge]
  ↓
[Generate M29 - Electronic Signature]
  ↓
[Generate M30 - URL Shortener]
  ↓
[Generate M31 - aiMarketing]
  ↓
[Generate M32 - CRM]
  ↓
[Generate M33 - E-Invoice]
  ↓
[Generate M34 - Payment Gateway]
  ↓
[Generate M35 - Digital Signature]
  ↓
[Generate M36 - Promotion Management]
  ↓
[Generate M37 - Store/POS]
  ↓
[Generate M38 - OneAI Platform]
  ↓
[Generate M39 - Operations Dashboard]
  ↓
[Generate M40 - Group Management]
  ↓
[Generate M41 - E-Banking]
  ↓
END - All 41 modules documented
```

---

## 📁 Expected Output Structure

After full automation, the repository will contain:

```
abn.erp.misa/
├── REQUIREMENTS.md                    # Master requirements (existing)
├── MASTER_INDEX.md                    # This file
├── MASTER_PROMPT.md                   # Master automation prompt
├── docs/
│   ├── M01_user_authentication.md
│   ├── M02_roles_permissions.md
│   ├── M03_company_organization.md
│   ├── M04_accounting.md
│   ├── M05_employee_management.md
│   ├── M06_task_management.md
│   ├── M07_purchasing.md
│   ├── M08_sales.md
│   ├── M09_inventory_warehouse.md
│   ├── M10_asset_management.md
│   ├── M11_contract_management.md
│   ├── M12_payroll.md
│   ├── M13_time_tracking.md
│   ├── M14_recruitment.md
│   ├── M15_performance_evaluation.md
│   ├── M16_objectives_kpi.md
│   ├── M17_social_insurance.md
│   ├── M18_labor_relations.md
│   ├── M19_workflow.md
│   ├── M20_internal_chat.md
│   ├── M21_document_management.md
│   ├── M22_correspondence.md
│   ├── M23_social_network.md
│   ├── M24_meeting_room_booking.md
│   ├── M25_contact_directory.md
│   ├── M26_video_library.md
│   ├── M27_photo_library.md
│   ├── M28_notes_knowledge.md
│   ├── M29_electronic_signature.md
│   ├── M30_url_shortener.md
│   ├── M31_aimarketing.md
│   ├── M32_crm.md
│   ├── M33_einvoice.md
│   ├── M34_payment_gateway.md
│   ├── M35_digital_signature.md
│   ├── M36_promotion_management.md
│   ├── M37_store_pos.md
│   ├── M38_oneai_platform.md
│   ├── M39_operations_dashboard.md
│   ├── M40_group_management.md
│   └── M41_ebanking.md
└── assets/                            # 73 screenshots (existing)
```

---

## ⚡ Execution Instructions

1. **Copy** the content of `MASTER_PROMPT.md`
2. **Paste** into your AI assistant
3. **Wait** for it to complete all 41 modules
4. **Save** each module output to its corresponding `docs/MXX_module_name.md` file
5. **Commit** to repository

Estimated total output: **~15,000-20,000 lines** of technical documentation across 41 files.

---

*Generated April 14, 2026*
