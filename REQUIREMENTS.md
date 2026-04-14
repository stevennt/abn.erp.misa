# ERP Application Requirements Document
## Complete MISA AMIS Clone Specification

**Source:** [GitHub Issue #632](https://github.com/stevennt/abn.launchpad/issues/632)  
**Repository:** stevennt/abn.launchpad  
**Author:** stevennt (ABN Software)  
**Created:** March 2026  
**Total Screenshots:** 73 images (all downloaded to `/assets`)  

---

## 📌 Executive Summary

This document contains the complete requirements for building an ERP application cloned from **MISA AMIS** - a comprehensive Vietnamese enterprise resource planning platform. The system integrates **all business operations on a single software platform** with modular architecture and AI-powered features.

**Key Vision:** "TẤT CẢ NGHIỆP VỤ KẾ TOÁN TRÊN 1 PHẦN MỀM" (All Accounting Operations on 1 Software)

---

## 🎯 Core ERP Modules Overview

### Module Map

![MISA AMIS Core Modules](./assets/image_01.png)

The system encompasses **10 major operational areas**:

1. **Phân tích tài chính** (Financial Analysis)
2. **Ngân hàng** (Banking)
3. **Mua hàng** (Purchasing)
4. **Bán hàng** (Sales)
5. **Quản lý hóa đơn** (Invoice Management)
6. **Thuế** (Tax)
7. **Hợp đồng** (Contracts)
8. **Kho, thủ kho** (Warehouse & Inventory)
9. **Công cụ dụng cụ** (Tools & Equipment)
10. **Tiền lương** (Payroll)
11. **Giá thành** (Costing)
12. **Ngân sách** (Budget)

---

## 🏢 Application Marketplace Architecture

### Platform Dashboard

![Platform Dashboard](./assets/image_03.png)

The ERP features a centralized dashboard with app categories:
- **Ứng dụng của tôi** (My Apps)
- **Tất cả** (All)
- **Tài chính** (Finance)
- **Kinh doanh** (Business)
- **Nhân sự** (HR)
- **Văn phòng số** (Digital Office)

### Complete Application Catalog

![Application Marketplace](./assets/image_04.png)

#### 1. TÀI CHÍNH (Finance) - 8 Apps
- **Kế toán** - Enterprise accounting
- **Kế toán HKD** - Business household accounting
- **Chữ ký số** - Digital signature service
- **Cổng thanh toán** - Payment gateway
- **Ngân hàng điện tử** - E-banking integration
- **Hóa đơn điện tử** - E-invoice issuance
- **Kế toán HTX** - Cooperative accounting
- **Quản lý tập đoàn** - Group management

#### 2. KINH DOANH (Business) - 4 Apps
- **aiMarketing** - Marketing automation with AI
- **CRM** - Customer relationship management
- **Khuyến mại** - Promotion management
- **Cửa hàng** - Store/retail management

#### 3. NHÂN SỰ (HR) - 10 Apps
- **Bộ giải pháp Quản trị nhân sự** - Complete HR solution
- **Tuyển dụng** - Recruitment & hiring
- **Thông tin nhân sự** - HR information system
- **Nhân viên** - Employee management
- **Mục tiêu** - Objective management (OKR/KPI)
- **Đánh giá** - Performance evaluation
- **Chấm công** - Time & attendance tracking
- **Tiền lương** - Payroll processing
- **Bảo hiểm xã hội** - Social insurance declaration
- **Thuế TNCN** - Personal income tax

#### 4. VĂN PHÒNG SỐ (Digital Office) - 17 Apps
- **Bộ giải pháp MISA** - MISA integrated solution
- **Điều hành** - Operations dashboard
- **Công việc** - Task management
- **Quy trình** - Workflow management
- **Tài liệu điện tử** - Electronic documents
- **Ghi chép** - Notes & documentation
- **Văn thư** - Correspondence management
- **Tài sản** - Asset management
- **Mạng xã hội** - Internal social network
- **Phòng họp** - Meeting room booking
- **Danh bạ** - Contact directory
- **Kho phim** - Video library
- **Kho ảnh** - Photo library
- **MISA ShortLink** - URL shortener
- **Chat** - Internal messaging
- **OneAI** - AI platform integration
- **Workflow** - Workflow automation

#### 5. CHUỖI CUNG ỨNG (Supply Chain) - 2 Apps
- **Mua hàng** - Purchasing management
- **Kho hàng** - Warehouse management

---

## 📋 Detailed Module Specifications

### 1. Asset Management (Quản lý tài sản)

#### Module Overview
![Asset Management Detail](./assets/image_08.png)

**Purpose:** Comprehensive tool and equipment asset management supporting management, operations, and asset administration across all departments.

#### Dashboard Features
![Asset Dashboard](./assets/image_11.png)

**Key Metrics:**
- **Biểu đồ tài sản theo tình trạng** (Asset Status Chart)
  - Tổng: 165 assets
  - Đang sử dụng: 60 (In use)
  - Chưa sử dụng: 30 (Not yet used)
  - Đang bảo dưỡng: 24 (Under maintenance)
  - Đang sửa chữa: 10 (Being repaired)
  - Đề nghị thanh lý: 5 (Proposed for disposal)
  - Đã hủy: 5 (Destroyed)
  - Đã mất: 5 (Lost)
  - Bảo mất: 5 (Insurance loss)
  - Đang điều chuyển: 3 (In transfer)
  - Báo hỏng: 10 (Damaged report)
  - Đã hỏng: 5 (Broken)

- **Biểu đồ biến động tài sản** (Asset Movement Chart)
  - Monthly quantity and value tracking
  - 12-month trend analysis

- **Biểu đồ tình hình sửa chữa tài sản** (Asset Repair Status)
  - Repair quantity and cost tracking
  - Monthly repair trends

- **Nhắc việc** (Reminders)
  - Hôm nay (Today): 5 tasks
  - Hôm qua (Yesterday): 3 tasks
  - 7 ngày gần nhất (Last 7 days): 2 tasks
  - Tháng này (This month): 4 tasks
  - Cũ hơn 6 tháng (Older than 6 months): 2 tasks

**Navigation Menu:**
- Tổng quan (Overview)
- Tài sản (Assets)
- Cấp phát - Thu hồi (Allocation - Recovery)
- Điều chuyển (Transfer)
- Sửa chữa - Bảo dưỡng (Repair - Maintenance)
- Mất - Hủy - Thanh lý (Loss - Destroy - Disposal)
- Kiểm kê (Inventory)
- Báo cáo (Reports)
- Thiết lập (Settings)

---

### 2. Task Management (Quản lý công việc)

#### Task Dashboard
![Task Dashboard](./assets/image_17.png)

**Features:**
- **Số lượng công việc hôm nay** (Today's Task Count)
  - Quá hạn (Overdue): 1
  - Đến hạn (Due): 2
  - Sắp đến hạn (Due Soon): 0
  - Chờ tôi duyệt (Awaiting My Approval): 0
  - Công việc cần giao (Tasks to Assign): 1

- **Việc của tôi - tuần này** (My Tasks - This Week)
  - Total: 16 tasks
  - Hoàn thành đúng hạn (On-time): 1
  - Hoàn thành trễ hạn (Late): 3
  - Chưa hoàn thành (Incomplete): 4
  - Chờ duyệt (Pending): 4
  - Quá hạn (Overdue): 4

- **Danh sách công việc hôm nay** (Today's Task List)
  - Priority levels: Khẩn cấp (Urgent), AMIS Mobile
  - Task details with assignees and deadlines

**Navigation:**
- Tổng quan (Overview)
- Việc của tôi (My Tasks)
- Báo cáo (Reports)
- Tìm kiếm dự án (Search Projects)
- Cá nhân (Personal)
- Khối vận hành (Operations Block)
  - Phòng Nhân sự (HR Department)
  - Phòng HCTH (Administration Department)

#### Task Processing in Chat
![Task Processing in AMIS Chat](./assets/image_22.png)

**Integration Features:**
- Create tasks directly from chat conversations
- @mention to assign tasks
- Real-time notifications
- Cross-application data sync
- Quick task creation with checklists
- File attachments in tasks

---

### 3. Marketing Automation (aiMarketing)

#### Marketing Dashboard
![aiMarketing Dashboard](./assets/image_09.png)

**Reports (Báo cáo):**
- **Doanh số phát sinh** (Generated Revenue): 40,435,500,000đ ↑15.2%
- **Chi phí** (Costs): 20,121,500,000đ ↑15.2%
- **ROI**: 28,55% ↑15.2%

**Performance Metrics:**
- **Hiệu quả chuyển đổi theo thời gian** (Conversion Performance Over Time)
  - Lead: 1,852,371
  - MQL: 1,239,091
  - SAL: 1,055,108
  - SQL: 1,012,242

- **Hiệu quả chăm sóc Khách hàng** (Customer Care Effectiveness)
  - Tỷ lệ phát sinh cơ hội (Opportunity Rate): 38% ↑15.2%
  - Tỷ lệ phát sinh doanh số (Revenue Rate): 25.1% ↑15.2%
  - AOV (Average Order Value): 58,950,000đ ↓7.2%

**Navigation:**
- Tổng quan (Overview)
- Chi phí (Costs)
- Chiến dịch (Campaigns)
- Lead
- Liên hệ (Contacts)
- Nhóm liên hệ (Contact Groups)
- Landing page
- Email
- Form
- Banner
- CTA
- Workflow
- URL tracking
- Keyword Research

---

### 4. Human Resources Management

#### Employee Database (Thông tin nhân sự)
![Employee Database](./assets/image_35.png)

**Employee Profile Fields:**
- Mã nhân viên (Employee ID): D02-0067
- Họ và đệm (First Name): Lục Phong
- Tên (Last Name): Vũ
- Giới tính (Gender): Nữ (Female)
- Ngày sinh (Date of Birth): 04/12/1995
- Nơi sinh (Place of Birth): Hà Nội
- Tình trạng hôn nhân (Marital Status): Độc thân (Single)
- MST cá nhân (Personal Tax ID): -
- TP gia đình (Family Type): Nông dân (Farmer)
- TP bản thân (Self Type): -
- Dân tộc (Ethnicity): Không (None)
- Tôn giáo (Religion): Không (None)

**Tabs:**
- Thông tin cơ bản (Basic Information)
- Thông tin liên hệ (Contact Information)
- Thông tin công việc (Work Information)
- Thông tin tài khoản (Account Information)
- Thông tin gia đình (Family Information)
- Khác (Other)

**Total Employees:** 100 (showing 1-50)

#### Objective Management (Mục tiêu)
![Objective Management](./assets/image_39.png)

**Features:**
- Organizational and personal objectives
- MBO (Management by Objectives)
- OKR (Objectives and Key Results)
- KPI (Key Performance Indicators)
- Progress tracking with visual bars
- Hierarchical objective structure

**Example Structure:**
```
Công ty cổ phần MISA
├── Khối sản xuất (Production Block)
├── Văn phòng Tổng công ty (Head Office)
├── Văn phòng MISA Hà Nội
├── Văn phòng MISA Đà Nẵng
├── Văn phòng MISA Buôn Mê Thuật
├── Văn phòng MISA Tp Hồ Chí Minh
└── Văn phòng MISA Cần Thơ
```

**Objective Types:**
- Mục tiêu doanh số toàn công ty năm 2023 (Company-wide revenue target 2023)
- Mục tiêu doanh số VP MISA HN năm 2023 (Hanoi Office revenue target)
- Hoàn thành kế hoạch doanh số TT KDDN (Complete business sales plan)
- Thực hiện tốt công tác tuyển dụng (Execute recruitment well)

**Status:** Chưa hoàn thành (Incomplete) with 50% progress

#### Performance Evaluation (Đánh giá)
![Evaluation Timeline](./assets/image_44.png)

**Gantt Chart View:**
- Monthly timeline view (Tháng 1-12)
- Objective start and end dates
- Visual progress bars
- Employee-specific objectives
- Regional breakdowns (Đông Bắc Bộ, Tây Nam Bộ, Nam Trung Bộ, etc.)

**Evaluation Methods:**
- Mục tiêu cá nhân (Personal Objectives)
- Đánh giá năng lực (Competency Evaluation)
- Báo cáo công việc (Work Reports)

**Navigation:**
- Phương pháp (Methods)
- Kỳ đánh giá (Evaluation Period)
- Thiết lập (Settings)
  - Nhân viên (Employees)
  - Tùy chỉnh (Customize)
  - Vai trò (Roles)
  - Người dùng (Users)
  - Hệ thống (System)

#### Evaluation Template Library
![Template Library](./assets/image_46.png)

**Available Templates:**
1. Mục tiêu mẫu cho bộ phận Nhân sự (HR Department Templates)
2. Mục tiêu mẫu cho bộ phận Marketing (Marketing Department Templates)
3. Mục tiêu mẫu cho bộ phận Bán hàng (Sales) (Sales Department Templates)
4. Mục tiêu mẫu cho bộ phận Chăm sóc khách hàng (Customer Care Templates)
5. Mục tiêu mẫu cho bộ phận Sản xuất (Production Department Templates)

---

### 5. Internal Communication & Collaboration

#### Photo Library (Kho ảnh)
![Photo Library](./assets/image_24.png)

**Features:**
- Social media-like interface
- Image viewing with navigation arrows
- Like, Comment, Share functionality
- Upload date tracking
- Comment threads with user profiles
- Engagement metrics (17 Thích, 34 Bình luận, 5 Chia sẻ)

#### Invoice Management Dashboard
![Invoice Dashboard](./assets/image_28.png)

**Features:**
- Total invoices issued statistics
- Invoice value breakdown by type
- Invoice timeline charts
- Mobile app integration
- Automated tax reporting

---

### 6. Financial Operations

#### Welcome Email Example
![Welcome Email](./assets/image_02.png)

**Onboarding Process:**
New employees receive an email with:
- Access to MISA AMIS platform
- Workflow management (AMIS Quy trình):
  1. Tạm ứng (Advance payment)
  2. Thanh toán (Payment)
  3. Thanh toán tạm ứng (Advance payment settlement)
  4. Quy trình mua hàng (Purchasing workflow)
  5. Duyệt báo giá hợp đồng tài liệu (Contract quotation approval)
  6. Duyệt đi muộn, về sớm, nghỉ phép (Late/early leave/leave approval)
- Internal communication via AMIS Chat
- AI integration via OneAI (ChatGPT, Gemini, Grok, Claude)

---

## 🤖 AI Features (OneAI Platform)

Based on issue comment (2026-04-13 07:25:09):

### Admin Features (Chức năng Quản trị)

**Resource Management:**
- **Phân quyền và quản lý người dùng** - Role-based access control with detailed permissions
- **Thiết lập định mức tài nguyên** - Resource quota settings to prevent abuse and optimize costs
- **Báo cáo tình hình sử dụng đa chiều** - Multi-dimensional usage reports by user, time, feature type, and AI model

### User Features (Chức năng cho Mọi Người dùng)

#### AI Chat (Trò chuyện với AI)
- **Đa mô hình AI** - Multiple AI models (GPT, Claude, Gemini, DeepSeek...)
- **Hỏi đáp thông tin** - Q&A, consulting, and decision support

#### Content Creation (Tạo nội dung)
- **Tạo ảnh** - Text-to-image generation with high quality
- **Tạo file** - Auto-export documents, spreadsheets, presentations
- **Nghiên cứu sâu** - Complex data analysis and information synthesis

#### Specialized Tools (Bộ công cụ chuyên biệt)

**10 AI-Powered Tools:**
1. **Sáng tạo nội dung** - Article writing, script creation, idea development
2. **Dịch thuật** - Multi-language translation (accurate and natural)
3. **Tóm tắt** - Document summarization to key points
4. **Phân tích tài liệu** - File reading and information extraction
5. **Soạn email** - Professional email generation
6. **Lập kế hoạch** - Roadmap building and resource allocation
7. **Lập báo cáo** - Automated report generation from raw data
8. **Hỗ trợ thiết kế** - UI/UX design consulting, layout, colors
9. **Trợ lý Excel** - Formula creation, spreadsheet data analysis
10. **Thư viện prompt** - Optimized prompt templates for each domain

#### Advanced Features (Tính năng nâng cao)
- **Tích hợp trình duyệt** - Browser integration for AI while browsing
- **Tạo bot tùy chỉnh** - Custom chatbot building

### Mobile Version (Phiên bản Mobile)

**Mobile Features:**
- **Trò chuyện AI di động** - Mobile AI chat with multi-model support anytime, anywhere
- **Tạo ảnh từ điện thoại** - AI image generation on mobile
- **Khám phá công cụ** - Full access to specialized tools
- **Đồng bộ đa thiết bị** - Cross-device sync with secure chat history and document storage

**App Download:** http://onelink.to/ezkz3e

---

## ️ Technical Architecture

### Platform Structure

**Frontend:**
- Web application with responsive design
- Mobile apps (iOS & Android)
- Desktop application
- Browser extension integration

**Backend:**
- Microservices architecture
- Modular app marketplace
- API-first design
- Multi-tenant support

**Database:**
- Relational database for transactional data
- Document storage for files and media
- Search indexing for quick retrieval

**Integration:**
- Third-party service APIs (banking, payment, digital signatures)
- AI model APIs (OpenAI, Anthropic, Google, DeepSeek)
- Single Sign-On (SSO)
- Digital signature services

### Security & Access Control

- Role-based permissions
- Resource usage quotas
- Activity logging and auditing
- Data encryption at rest and in transit
- Multi-factor authentication

---

## 📊 Key Metrics & Reporting

Each module includes comprehensive reporting:

### Financial Reports
- Revenue and cost tracking
- Budget vs. actual analysis
- Cash flow statements
- Tax reports

### HR Reports
- Employee performance metrics
- Attendance and overtime
- Payroll summaries
- Recruitment pipeline analytics

### Operations Reports
- Task completion rates
- Asset utilization
- Workflow efficiency
- Inventory turnover

### Marketing Reports
- Campaign ROI
- Lead conversion funnels
- Customer acquisition costs
- Channel performance

---

## 📱 Mobile Application

The mobile app provides full functionality:
- Native iOS and Android apps
- Offline mode for key features
- Push notifications
- Biometric authentication
- Mobile-optimized UI

**Screenshot Gallery (Images 67-73):**
- Login and authentication screens
- Dashboard views
- Module navigation
- Data entry forms
- Reports and analytics
- Settings and profile management

---

## 📦 Implementation Roadmap

### Phase 1: Core Accounting & Finance
- [ ] General ledger
- [ ] Accounts payable/receivable
- [ ] Banking integration
- [ ] Invoice management
- [ ] Tax reporting

### Phase 2: HR Management
- [ ] Employee database
- [ ] Recruitment module
- [ ] Time & attendance
- [ ] Payroll processing
- [ ] Performance evaluation

### Phase 3: Business Operations
- [ ] Sales management
- [ ] Purchasing
- [ ] Inventory/warehouse
- [ ] Contract management
- [ ] Asset management

### Phase 4: Digital Office
- [ ] Task management
- [ ] Workflow automation
- [ ] Internal communication
- [ ] Document management
- [ ] Meeting room booking

### Phase 5: AI Integration
- [ ] OneAI platform setup
- [ ] Multi-model AI integration
- [ ] Content creation tools
- [ ] Specialized AI assistants
- [ ] Mobile AI features

### Phase 6: Analytics & Reporting
- [ ] Dashboard creation
- [ ] Custom report builder
- [ ] Data visualization
- [ ] Export functionality
- [ ] Scheduled reports

---

## 📁 Asset Inventory

All 73 screenshots have been downloaded to the `/assets` folder:

| File | Description | Size |
|------|-------------|------|
| image_01.png | Core ERP modules overview | 348 KB |
| image_02.png | Welcome email template | 86 KB |
| image_03.png | Platform dashboard | 286 KB |
| image_04.png | Application marketplace catalog | 213 KB |
| image_05.png | Asset management detail | 758 KB |
| image_06.png | Task management overview | 692 KB |
| image_07.png | Marketing automation | 486 KB |
| image_08.png | Asset management product page | 200 KB |
| image_09.png | aiMarketing dashboard | 130 KB |
| image_10.png | HR module overview | 144 KB |
| image_11.png | Asset dashboard with charts | 123 KB |
| ... | ... | ... |
| image_73.png | Mobile app features | 327 KB |

**Total Images:** 73  
**Total Size:** ~15 MB  

---

## 📞 Additional Resources

- **Google Mail Reference:** FMfcgzQgLPMRnWRTSxxtWKxTKlhDCgrP
- **App Download Link:** http://onelink.to/ezkz3e
- **GitHub Issue:** https://github.com/stevennt/abn.launchpad/issues/632

---

## ✅ Next Steps

1. **Review all 73 screenshots** in the `/assets` folder
2. **Prioritize modules** based on business needs
3. **Define technical stack** and architecture
4. **Create detailed user stories** for each module
5. **Set up development environment**
6. **Begin Phase 1 implementation** (Core Accounting)

---

*Document generated from GitHub Issue #632 on April 14, 2026*
