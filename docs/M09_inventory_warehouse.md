# M09 Inventory & Warehouse Module Specification

## 1. Module Overview

### 1.1 Purpose

The Inventory & Warehouse Module (M09) is the core stock management system within the ABN ERP platform. It provides comprehensive inventory control including warehouse management, location tracking, stock movement recording, stock adjustments, inter-warehouse transfers, stock counting, stock alerts, and warehouse keeper assignment. The module ensures accurate real-time stock visibility and supports all inventory-related operations across the organization.

The core entity "Kho thủ kho" (Warehouse-Keeper) represents the primary warehouse management unit with assigned keepers responsible for stock accuracy. The module handles stock items across multiple locations, tracks all stock movements (receipts, issues, transfers, adjustments), manages stock alerts for minimum/maximum levels, and supports periodic stock counting with variance analysis.

### 1.2 Personas

| Persona | Role | Key Responsibilities | Primary Screens | Access Level |
|---------|------|---------------------|-----------------|--------------|
| Warehouse Manager | Department leadership | Oversee all warehouse operations, approve adjustments | Dashboard, Reports, Settings | Full |
| Warehouse Keeper | Stock management | Daily stock operations, receipts, issues, transfers | Stock Movement, Stock Count | Read/Write |
| Inventory Controller | Stock optimization | Stock level monitoring, reorder management, alerts | Stock Alert, Stock Level | Read/Write |
| Stock Counter | Physical counting | Conduct stock counts, record variances | Stock Count | Read/Write |
| Receiving Clerk | Goods receipt | Receive goods from suppliers, inspect, put away | Goods Receipt, Putaway | Read/Write |
| Shipping Clerk | Goods dispatch | Pick, pack, and ship orders | Goods Issue, Shipment | Read/Write |
| Finance Officer | Inventory valuation | Stock valuation, COGS tracking | Valuation Reports | Read-only |
| Director | Strategic oversight | Inventory trends, stock investment analysis | Reports, Dashboard | Read-only |

### 1.3 Dependencies

| Dependency | Type | Direction | Description |
|-----------|------|-----------|-------------|
| M04 Accounting | Downstream | Outbound | Stock movements -> GL entries, inventory valuation |
| M07 Purchasing | Upstream | Inbound | Purchase orders -> Goods receipt |
| M08 Sales | Upstream | Inbound | Sale orders -> Stock deduction |
| M10 Asset Management | Peer | Inbound | Equipment tracking in warehouse |
| M05 Employee Mgmt | Peer | Inbound | Employee assignment as warehouse keepers |
| M06 Task Mgmt | Peer | Inbound | Warehouse tasks and maintenance |
| Barcode/RFID System | External | Inbound | Stock tracking hardware |
| Weighing/Measurement | External | Inbound | Physical measurement devices |

### 1.4 Business Rules

1. **Warehouse-Keeper Assignment**: Every warehouse must have at least one designated keeper responsible for stock accuracy.
2. **Stock Movement Recording**: All stock movements must be recorded with source, destination, quantity, and reference document.
3. **Negative Stock Prevention**: Stock cannot go below zero; system blocks issue transactions when insufficient stock.
4. **FIFO Valuation**: Inventory valuation uses FIFO (First In, First Out) method unless weighted average is configured.
5. **Stock Count Frequency**: Full stock count required quarterly; cycle counts weekly for high-value items.
6. **Variance Threshold**: Stock count variances exceeding 2% or 500K VND require investigation and manager approval.
7. **Alert Thresholds**: Each stock item can have minimum and maximum stock levels; alerts triggered when breached.
8. **Location Tracking**: Stock tracked at warehouse and location (bin/shelf) level for accurate physical tracking.
9. **Batch/Lot Tracking**: Products with batch requirements (expiry dates) tracked by batch/lot number.
10. **Transfer Authorization**: Inter-warehouse transfers require authorization from both source and destination keepers.
11. **Adjustment Approval**: Stock adjustments (write-offs, corrections) require manager approval above threshold.
12. **Unit of Measure**: All stock quantities stored in base unit; conversions for purchase/sale units supported.

---

## 2. Screen Inventory

### 2.1 Screen Routes and Purposes

| Route | Purpose | Personas | Priority |
|-------|---------|----------|----------|
| `/warehouse/dashboard` | Warehouse dashboard with KPIs and alerts | All personas | Critical |
| `/warehouse/warehouses` | Warehouse list and management | Warehouse Manager | High |
| `/warehouse/warehouses/{id}` | Warehouse detail with locations and keeper | Warehouse Manager | High |
| `/warehouse/locations` | Location/bin management | Warehouse Manager, Keeper | High |
| `/warehouse/stock-items` | Current stock levels by warehouse/location | Inventory Controller | Critical |
| `/warehouse/stock-items/{product_id}` | Product stock detail across warehouses | Inventory Controller | High |
| `/warehouse/movements` | Stock movement history | Warehouse Keeper | Critical |
| `/warehouse/movements/new` | Record new stock movement (receipt/issue) | Warehouse Keeper | Critical |
| `/warehouse/adjustments` | Stock adjustment management | Warehouse Manager | High |
| `/warehouse/transfers` | Inter-warehouse transfer management | Warehouse Keeper | High |
| `/warehouse/stock-counts` | Stock count sessions and results | Stock Counter, Keeper | High |
| `/warehouse/stock-counts/{id}` | Stock count detail with variance analysis | Stock Counter | High |
| `/warehouse/stock-alerts` | Stock alert list and management | Inventory Controller | High |
| `/warehouse/reports` | Inventory reports: valuation, movement, turnover | Warehouse Manager, Director | High |
| `/warehouse/keepers` | Warehouse keeper assignment | Warehouse Manager | Medium |

### 2.2 UI Components by Screen

#### /warehouse/dashboard

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Total SKUs KPI | Metric Card | Total unique products in stock | StockItem count |
| Total Value KPI | Metric Card | Total inventory value | StockItem valuation |
| Low Stock Alerts KPI | Metric Card | Items below minimum | StockAlert WHERE low |
| Over Stock Alerts KPI | Metric Card | Items above maximum | StockAlert WHERE over |
| Pending Receipts KPI | Metric Card | Expected incoming stock | StockMovement WHERE pending receipt |
| Pending Issues KPI | Metric Card | Pending outbound stock | StockMovement WHERE pending issue |
| Stock Level Chart | Bar Chart | Stock quantity by product | StockItem aggregation |
| Movement Trend Chart | Line Chart | Monthly stock movements | Movement history |
| Warehouse Utilization | Gauge Chart | Space utilization per warehouse | Location occupancy |
| Alert Panel | List | Active stock alerts | StockAlert |
| Quick Actions Bar | Action Bar | New movement, new count, transfer | N/A |

#### /warehouse/stock-items

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Search Bar | Input | Search by product name, code, barcode | Product search |
| Warehouse Filter | Dropdown | Filter by warehouse | Warehouse list |
| Category Filter | Multi-select | Filter by product category | ProductCategory |
| Stock Grid | Data Table | Current stock levels | StockItem |
| Quantity Column | Number | Current stock quantity | StockItem.quantity |
| Value Column | Currency | Stock value | StockItem.quantity * cost |
| Min/Max Columns | Number | Min/max thresholds | StockItem thresholds |
| Status Badge | Badge | OK, Low, Over, Zero | StockAlert status |
| Export Button | Action Button | Export stock report | StockItem data |

#### /warehouse/stock-counts/{id}

| Component | Type | Purpose | Data Source |
|-----------|------|---------|-------------|
| Count Header | Info Panel | Count session info, warehouse, date | StockCount |
| Item Grid | Data Table | Items to count with system qty | StockCountItem |
| System Quantity | Number | System-recorded quantity | StockItem.quantity |
| Counted Quantity | Input | Physical count entry | User input |
| Variance Column | Number | Counted - System | Calculated |
| Variance % Column | Percentage | Variance percentage | Calculated |
| Status Badge | Badge | Not counted, counted, variance OK, variance high | StockCountItem status |
| Submit Button | Action Button | Submit count for review | StockCount |
| Approve Button | Action Button | Approve and adjust stock | Warehouse Manager |

### 2.3 User Interactions

1. **Goods Receipt**: Warehouse Keeper receives goods from purchase order -> records receipt with quantity -> system validates against PO -> updates stock quantity at assigned location -> creates stock movement record (receipt).
2. **Stock Count**: Stock Counter starts count session for warehouse -> walks through locations -> counts physical stock -> enters counted quantities -> system calculates variances -> flags items exceeding threshold -> Warehouse Manager reviews and approves -> system adjusts stock levels.
3. **Inter-Warehouse Transfer**: Warehouse Keeper creates transfer request -> selects source and destination warehouses -> selects items and quantities -> source keeper confirms issue -> destination keeper confirms receipt -> stock updated at both warehouses.
4. **Stock Alert Response**: Inventory Controller sees low stock alert -> creates purchase request -> tracks receipt -> alert cleared when stock above minimum.

### 2.4 Data Displayed

- Stock Item: Product code, name, warehouse, location, quantity, unit, cost price, total value, min level, max level, status
- Stock Movement: Movement number, date, type (receipt/issue/transfer/adjustment), product, quantity, from/to, reference document, status
- Stock Count: Count session number, warehouse, date, item count, items counted, variances, status
- Stock Alert: Product, warehouse, current qty, min qty, max qty, alert type (low/over/zero), date triggered

---

## 3. Feature Specifications

### 3.1 Warehouse and Location Management

**Description**: Define warehouses, locations (bins/shelves), and assign warehouse keepers.

**User Stories**:
- As a Warehouse Manager, I want to define warehouses with multiple locations so that stock can be precisely tracked.
- As a Warehouse Manager, I want to assign keepers to warehouses so that responsibility is clear.
- As a Warehouse Keeper, I want to see my assigned warehouses so that I know my scope of responsibility.

**Acceptance Criteria**:
1. Warehouse creation with: code, name, address, type (main, branch, virtual), status.
2. Location hierarchy: Warehouse -> Zone -> Aisle -> Bin -> Shelf.
3. Warehouse keeper assignment with effective dates.
4. Location capacity tracking (volume, weight).
5. Location status: active, reserved, maintenance.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-001 | Warehouse Code | Required, unique | "Warehouse code is required and must be unique" |
| VR-002 | Keeper | At least one keeper per warehouse | "Assign at least one keeper" |
| VR-003 | Location Code | Required, unique within warehouse | "Location code must be unique within warehouse" |
| VR-004 | Capacity | Must be > 0 if tracking | "Capacity must be positive" |
| VR-005 | Status | Valid status value | "Invalid location status" |

**Edge Cases**:
- Warehouse closure: All stock must be transferred out before closure.
- Keeper reassignment: Previous keeper's responsibility end date tracked.
- Location reorganization: Bins can be merged or split with stock relocation.

### 3.2 Stock Movement Management

**Description**: Record and track all stock movements including receipts, issues, transfers, and adjustments.

**User Stories**:
- As a Warehouse Keeper, I want to record goods receipts from purchase orders so that stock levels are updated.
- As a Warehouse Keeper, I want to record stock issues for sale orders so that inventory is deducted.
- As an Inventory Controller, I want to see all stock movements so that I can trace stock history.

**Acceptance Criteria**:
1. Movement types: receipt (from supplier, return, transfer-in), issue (to customer, return, transfer-out, adjustment), transfer, adjustment.
2. Each movement references a source document (PO, SO, transfer, count).
3. Movement recording updates stock quantity in real-time.
4. Movement history viewable per product, warehouse, or document.
5. Batch/lot tracking for applicable products.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-010 | Movement Type | Required, valid type | "Valid movement type is required" |
| VR-011 | Product | Required, active product | "Product is required and must be active" |
| VR-012 | Quantity | Must be > 0 | "Quantity must be positive" |
| VR-013 | Warehouse | Required, active warehouse | "Valid warehouse is required" |
| VR-014 | Issue Qty | Cannot exceed available stock | "Insufficient stock for this issue" |
| VR-015 | Reference | Required for audit | "Reference document is required" |

**Edge Cases**:
- Backdated movement: Allowed within open period with manager approval.
- Movement reversal: Create opposite movement with reference to original.
- Partial receipt: Record partial quantity against PO line.

### 3.3 Stock Count Management

**Description**: Conduct physical stock counts, record variances, and adjust system stock levels.

**User Stories**:
- As a Stock Counter, I want to create count sessions for specific warehouses so that I can organize physical counts.
- As a Stock Counter, I want to enter counted quantities so that variances are calculated.
- As a Warehouse Manager, I want to review and approve variances so that stock levels are accurately adjusted.

**Acceptance Criteria**:
1. Count session creation with: warehouse, date range, scope (all items, specific category, specific items).
2. Count sheet generation with system quantities for comparison.
3. Blind count option: system quantities hidden from counter.
4. Variance calculation and flagging based on threshold.
5. Approval workflow: counter records -> keeper reviews -> manager approves -> stock adjusted.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-020 | Scope | At least one item or category | "Select items or category to count" |
| VR-021 | Counted Qty | Must be >= 0 | "Counted quantity cannot be negative" |
| VR-022 | Variance | > threshold requires reason | "Variance explanation required" |
| VR-023 | Session Status | Cannot adjust before approval | "Count must be approved before adjustment" |
| VR-024 | Duplicate Count | No overlapping sessions for same items | "Items already in active count session" |

**Edge Cases**:
- Partial count: Count only a subset of items in a session.
- Re-count: Re-count items with significant variances.
- Count during operations: Freeze stock movements during count or track movements separately.

### 3.4 Stock Alert Management

**Description**: Monitor stock levels against defined thresholds and generate alerts for low stock, over stock, and zero stock situations.

**User Stories**:
- As an Inventory Controller, I want to see items below minimum stock so that I can reorder.
- As an Inventory Controller, I want to see items above maximum stock so that I can reduce ordering.
- As a Warehouse Manager, I want configurable alert thresholds so that alerts are relevant.

**Acceptance Criteria**:
1. Alert types: low stock (below minimum), over stock (above maximum), zero stock (out of stock), expiring (batch expiry).
2. Threshold configuration per product per warehouse.
3. Real-time alert generation on stock change.
4. Alert resolution tracking: open, acknowledged, resolved.
5. Auto-create purchase request for low stock items.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-030 | Min Level | Must be >= 0 | "Minimum level cannot be negative" |
| VR-031 | Max Level | Must be >= min level | "Maximum must be >= minimum" |
| VR-032 | Alert Type | Valid type | "Valid alert type is required" |
| VR-033 | Product | Required | "Product is required" |

**Edge Cases**:
- Seasonal products: Different thresholds for different seasons.
- New products: Default thresholds from category until specific thresholds set.
- Alert suppression: Temporarily suppress alerts during known supply chain disruptions.

### 3.5 Inter-Warehouse Transfer

**Description**: Transfer stock between warehouses with issue and receipt confirmation.

**User Stories**:
- As a Warehouse Keeper, I want to transfer stock to another warehouse so that inventory is balanced across locations.
- As a Warehouse Keeper, I want to confirm receipt of transferred stock so that destination stock is updated.
- As a Warehouse Manager, I want to track transfer history so that I can audit stock movements.

**Acceptance Criteria**:
1. Transfer creation with: source warehouse, destination warehouse, items, quantities.
2. Two-step process: issue from source -> receipt at destination.
3. In-transit tracking: stock in transit between warehouses.
4. Transfer receipt with quantity verification.
5. Discrepancy handling: received quantity differs from issued.

**Validation Rules**:

| Rule | Field | Condition | Error Message |
|------|-------|-----------|---------------|
| VR-040 | Source != Destination | Must be different warehouses | "Source and destination must differ" |
| VR-041 | Source Stock | Sufficient stock at source | "Insufficient stock at source warehouse" |
| VR-042 | Receipt Qty | Cannot exceed issued quantity | "Received exceeds issued" |
| VR-043 | Both Keepers | Both keepers must confirm | "Both keepers must confirm transfer" |

**Edge Cases**:
- Transfer in transit: Stock deducted from source, not yet at destination.
- Partial transfer: Ship partial quantity, remainder cancelled or deferred.
- Transfer rejection: Destination rejects transfer, stock returned to source.

---

## 4. Database Design

### 4.1 Entity Relationship Diagram

```
+-------------------+       +-------------------+       +-------------------+
|    Warehouse      |       |    Location       |       |  WarehouseKeeper  |
+-------------------+       +-------------------+       +-------------------+
| PK warehouse_id   |<----->| PK location_id    |       | PK keeper_id      |
| warehouse_code    |       | location_code     |       | FK warehouse_id   |
| warehouse_name    |       | location_name     |       | FK user_id        |
| address           |       | FK warehouse_id   |       | FK zone_id        |
| warehouse_type    |       | FK parent_id      |       | start_date        |
| status            |       | location_type     |       | end_date          |
| capacity_volume   |       | capacity_volume   |       | is_primary        |
| capacity_weight   |       | capacity_weight   |       | status            |
+-------------------+       | status            |       +-------------------+
                            +-------------------+

+-------------------+       +-------------------+       +-------------------+
|   StockItem       |       |  StockMovement    |       | StockAdjustment   |
+-------------------+       +-------------------+       +-------------------+
| PK stock_id       |       | PK movement_id    |       | PK adjustment_id  |
| FK product_id     |       | movement_number   |       | adjustment_number |
| FK warehouse_id   |       | FK movement_type  |       | FK warehouse_id   |
| FK location_id    |       | FK product_id     |       | FK product_id     |
| quantity          |       | FK warehouse_id   |       | quantity          |
| reserved_quantity |       | FK location_id    |       | reason            |
| available_quantity|       | quantity          |       | FK reference_id   |
| cost_price        |       | FK from_location  |       | cost_impact       |
| last_count_date   |       | FK to_location    |       | status            |
| min_level         |       | FK reference_type |       | FK approved_by    |
| max_level         |       | reference_number  |       | approved_date     |
+-------------------+       | movement_date     |       +-------------------+
                            | FK created_by     |
                            +-------------------+       +-------------------+
                                    |                   |   StockTransfer   |
                    +-------------------------------+   +-------------------+
                    |     StockCount               |   | PK transfer_id    |
                    +-------------------------------+   | transfer_number   |
                    | PK count_id                   |   | FK source_wh_id   |
                    | count_number                  |   | FK dest_wh_id     |
                    | FK warehouse_id               |   | transfer_date     |
                    | count_date                    |   | status            |
                    | FK started_by                 |   | FK issued_by      |
                    | FK approved_by                |   | FK received_by    |
                    | status                        |   | notes             |
                    | count_type                    |   +-------------------+
                    +-------------------------------+
                            |
                            v
                    +-------------------------------+
                    |     StockCountItem            |
                    +-------------------------------+
                    | PK count_item_id              |
                    | FK count_id                   |
                    | FK stock_id                   |
                    | system_quantity               |
                    | counted_quantity              |
                    | variance                      |
                    | variance_pct                  |
                    | status                        |
                    | notes                         |
                    +-------------------------------+

+-------------------+       +-------------------+
|   StockAlert      |       |   StockBatch      |
+-------------------+       +-------------------+
| PK alert_id       |       | PK batch_id       |
| FK product_id     |       | FK product_id     |
| FK warehouse_id   |       | batch_number      |
| alert_type        |       | manufacture_date  |
| current_quantity  |       | expiry_date       |
| min_level         |       | quantity          |
| max_level         |       | FK warehouse_id   |
| triggered_date    |       | status            |
| status            |       +-------------------+
| resolved_date     |
+-------------------+
```

### 4.2 Table Schemas

#### Warehouse

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| warehouse_id | BIGINT | PK, SERIAL | Unique warehouse ID |
| warehouse_code | VARCHAR(20) | NOT NULL, UNIQUE | Warehouse code |
| warehouse_name | NVARCHAR(100) | NOT NULL | Warehouse name |
| address | NVARCHAR(300) | NULL | Warehouse address |
| warehouse_type | VARCHAR(20) | NOT NULL | main, branch, virtual, cold_storage |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, inactive, closed |
| capacity_volume | DECIMAL(12,2) | NULL | Capacity in cubic meters |
| capacity_weight | DECIMAL(12,2) | NULL | Capacity in kg |
| manager_id | BIGINT | FK REFERENCES User | Warehouse manager |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### Location

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| location_id | BIGINT | PK, SERIAL | Unique location ID |
| location_code | VARCHAR(30) | NOT NULL | Location code (e.g., A-01-02) |
| location_name | NVARCHAR(100) | NULL | Location name |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Parent warehouse |
| FK parent_id | BIGINT | NULL, FK REFERENCES Location | Parent location (zone/aisle) |
| location_type | VARCHAR(20) | NOT NULL | zone, aisle, bin, shelf |
| capacity_volume | DECIMAL(10,2) | NULL | Location capacity (cbm) |
| capacity_weight | DECIMAL(10,2) | NULL | Location capacity (kg) |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, reserved, maintenance |
| current_utilization | DECIMAL(5,2) | DEFAULT 0 | Current utilization % |

#### WarehouseKeeper

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| keeper_id | BIGINT | PK, SERIAL | Unique keeper ID |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Assigned warehouse |
| FK user_id | BIGINT | NOT NULL, FK REFERENCES User | Assigned user |
| start_date | DATE | NOT NULL | Assignment start date |
| end_date | DATE | NULL | Assignment end date |
| is_primary | BOOLEAN | NOT NULL, DEFAULT FALSE | Primary keeper flag |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, inactive, completed |

#### StockItem

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| stock_id | BIGINT | PK, SERIAL | Unique stock ID |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Warehouse |
| FK location_id | BIGINT | FK REFERENCES Location | Specific location |
| quantity | DECIMAL(14,4) | NOT NULL, DEFAULT 0 | Current quantity |
| reserved_quantity | DECIMAL(14,4) | NOT NULL, DEFAULT 0 | Reserved quantity |
| available_quantity | DECIMAL(14,4) | CALCULATED | quantity - reserved |
| cost_price | DECIMAL(18,2) | NOT NULL | Average cost price |
| total_value | DECIMAL(18,2) | CALCULATED | quantity * cost_price |
| last_count_date | DATE | NULL | Last count date |
| min_level | DECIMAL(14,4) | DEFAULT 0 | Minimum stock level |
| max_level | DECIMAL(14,4) | DEFAULT 0 | Maximum stock level (0 = unlimited) |
| reorder_point | DECIMAL(14,4) | DEFAULT 0 | Reorder point |
| updated_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Last update timestamp |

#### StockMovement

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| movement_id | BIGINT | PK, SERIAL | Unique movement ID |
| movement_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated movement number |
| movement_type | VARCHAR(20) | NOT NULL | receipt, issue, transfer_in, transfer_out, adjustment_in, adjustment_out, count_adjustment |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Warehouse |
| FK location_id | BIGINT | FK REFERENCES Location | Location |
| quantity | DECIMAL(14,4) | NOT NULL | Movement quantity |
| unit_cost | DECIMAL(18,2) | NOT NULL | Unit cost at movement |
| total_cost | DECIMAL(18,2) | NOT NULL | Total cost |
| FK from_location_id | BIGINT | NULL, FK REFERENCES Location | Source location (for transfers) |
| FK to_location_id | BIGINT | NULL, FK REFERENCES Location | Destination location |
| reference_type | VARCHAR(30) | NOT NULL | PO, SO, transfer, count, adjustment |
| reference_id | BIGINT | NOT NULL | Reference document ID |
| reference_number | VARCHAR(30) | NOT NULL | Reference document number |
| movement_date | DATE | NOT NULL | Movement date |
| batch_number | VARCHAR(30) | NULL | Batch/lot number |
| notes | NVARCHAR(300) | NULL | Movement notes |
| FK created_by | BIGINT | FK REFERENCES User | User who recorded |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### StockCount

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| count_id | BIGINT | PK, SERIAL | Unique count ID |
| count_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated count number |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Warehouse being counted |
| count_date | DATE | NOT NULL | Count date |
| count_type | VARCHAR(20) | NOT NULL | full, cycle, spot, annual |
| scope_type | VARCHAR(20) | NOT NULL | all, category, specific |
| scope_value | JSONB | NULL | Category IDs or item IDs |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'in_progress' | in_progress, completed, approved, adjusted |
| FK started_by | BIGINT | FK REFERENCES User | Count initiator |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_date | DATE | NULL | Approval date |
| total_items | INT | DEFAULT 0 | Total items to count |
| counted_items | INT | DEFAULT 0 | Items counted |
| items_with_variance | INT | DEFAULT 0 | Items with variance |
| notes | TEXT | NULL | Count notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### StockCountItem

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| count_item_id | BIGINT | PK, SERIAL | Unique count item ID |
| FK count_id | BIGINT | NOT NULL, FK REFERENCES StockCount | Count session |
| FK stock_id | BIGINT | NOT NULL, FK REFERENCES StockItem | Stock item |
| system_quantity | DECIMAL(14,4) | NOT NULL | System quantity |
| counted_quantity | DECIMAL(14,4) | NULL | Physical count quantity |
| variance | DECIMAL(14,4) | CALCULATED | counted - system |
| variance_pct | DECIMAL(5,2) | CALCULATED | Variance percentage |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, counted, variance_ok, variance_high |
| notes | NVARCHAR(300) | NULL | Counter notes |
| FK counted_by | BIGINT | NULL, FK REFERENCES User | Counter |
| counted_at | TIMESTAMP | NULL | Count timestamp |

#### StockAdjustment

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| adjustment_id | BIGINT | PK, SERIAL | Unique adjustment ID |
| adjustment_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated number |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Warehouse |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| quantity | DECIMAL(14,4) | NOT NULL | Adjustment quantity (+/-) |
| reason | VARCHAR(200) | NOT NULL | Adjustment reason |
| reference_type | VARCHAR(30) | NULL | count, damage, loss, correction |
| reference_id | BIGINT | NULL | Reference document ID |
| cost_impact | DECIMAL(18,2) | NOT NULL | Financial impact |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'pending' | pending, approved, rejected |
| FK approved_by | BIGINT | NULL, FK REFERENCES User | Approver |
| approved_date | TIMESTAMP | NULL | Approval timestamp |
| notes | TEXT | NULL | Additional notes |
| FK created_by | BIGINT | FK REFERENCES User | Creator |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### StockTransfer

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| transfer_id | BIGINT | PK, SERIAL | Unique transfer ID |
| transfer_number | VARCHAR(20) | NOT NULL, UNIQUE | Auto-generated number |
| FK source_warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Source warehouse |
| FK dest_warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Destination warehouse |
| transfer_date | DATE | NOT NULL | Transfer date |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'draft' | draft, issued, in_transit, received, completed, cancelled |
| FK issued_by | BIGINT | FK REFERENCES User | Issuing keeper |
| issued_date | TIMESTAMP | NULL | Issue timestamp |
| FK received_by | BIGINT | NULL, FK REFERENCES User | Receiving keeper |
| received_date | TIMESTAMP | NULL | Receipt timestamp |
| notes | TEXT | NULL | Transfer notes |
| created_at | TIMESTAMP | NOT NULL, DEFAULT NOW() | Creation timestamp |

#### StockAlert

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| alert_id | BIGINT | PK, SERIAL | Unique alert ID |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Warehouse |
| alert_type | VARCHAR(20) | NOT NULL | low_stock, over_stock, zero_stock, expiring |
| current_quantity | DECIMAL(14,4) | NOT NULL | Quantity at alert time |
| min_level | DECIMAL(14,4) | NOT NULL | Minimum threshold |
| max_level | DECIMAL(14,4) | NULL | Maximum threshold |
| triggered_date | TIMESTAMP | NOT NULL, DEFAULT NOW() | Alert triggered |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'open' | open, acknowledged, resolved |
| resolved_date | TIMESTAMP | NULL | Resolution timestamp |
| resolved_by | BIGINT | NULL, FK REFERENCES User | User who resolved |
| resolution_notes | NVARCHAR(300) | NULL | Resolution notes |

#### StockBatch

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| batch_id | BIGINT | PK, SERIAL | Unique batch ID |
| FK product_id | BIGINT | NOT NULL, FK REFERENCES Product | Product |
| batch_number | VARCHAR(30) | NOT NULL | Batch/lot number |
| manufacture_date | DATE | NULL | Manufacture date |
| expiry_date | DATE | NULL | Expiry date |
| quantity | DECIMAL(14,4) | NOT NULL | Batch quantity |
| FK warehouse_id | BIGINT | NOT NULL, FK REFERENCES Warehouse | Warehouse |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'active' | active, expired, quarantined |

### 4.3 Indexes

```sql
-- Warehouse indexes
CREATE INDEX idx_warehouse_code ON Warehouse(warehouse_code);
CREATE INDEX idx_warehouse_status ON Warehouse(status);
CREATE INDEX idx_warehouse_type ON Warehouse(warehouse_type);

-- Location indexes
CREATE UNIQUE INDEX idx_location_warehouse_code ON Location(warehouse_id, location_code);
CREATE INDEX idx_location_parent ON Location(parent_id);
CREATE INDEX idx_location_type ON Location(location_type);
CREATE INDEX idx_location_status ON Location(status);

-- WarehouseKeeper indexes
CREATE INDEX idx_keeper_warehouse ON WarehouseKeeper(warehouse_id);
CREATE INDEX idx_keeper_user ON WarehouseKeeper(user_id);
CREATE INDEX idx_keeper_active ON WarehouseKeeper(warehouse_id) WHERE status = 'active';

-- StockItem indexes
CREATE UNIQUE INDEX idx_stock_product_warehouse ON StockItem(product_id, warehouse_id);
CREATE INDEX idx_stock_warehouse ON StockItem(warehouse_id);
CREATE INDEX idx_stock_location ON StockItem(location_id);
CREATE INDEX idx_stock_quantity ON StockItem(quantity) WHERE quantity < min_level;
CREATE INDEX idx_stock_available ON StockItem(available_quantity);

-- StockMovement indexes
CREATE INDEX idx_movement_number ON StockMovement(movement_number);
CREATE INDEX idx_movement_product ON StockMovement(product_id);
CREATE INDEX idx_movement_warehouse ON StockMovement(warehouse_id);
CREATE INDEX idx_movement_type ON StockMovement(movement_type);
CREATE INDEX idx_movement_date ON StockMovement(movement_date);
CREATE INDEX idx_movement_reference ON StockMovement(reference_type, reference_id);
CREATE INDEX idx_movement_batch ON StockMovement(batch_number);

-- StockCount indexes
CREATE INDEX idx_count_number ON StockCount(count_number);
CREATE INDEX idx_count_warehouse ON StockCount(warehouse_id);
CREATE INDEX idx_count_date ON StockCount(count_date);
CREATE INDEX idx_count_status ON StockCount(status);

-- StockCountItem indexes
CREATE UNIQUE INDEX idx_count_item_unique ON StockCountItem(count_id, stock_id);
CREATE INDEX idx_count_item_stock ON StockCountItem(stock_id);
CREATE INDEX idx_count_item_status ON StockCountItem(status);
CREATE INDEX idx_count_item_variance ON StockCountItem(variance) WHERE ABS(variance) > 0;

-- StockAdjustment indexes
CREATE INDEX idx_adjustment_number ON StockAdjustment(adjustment_number);
CREATE INDEX idx_adjustment_warehouse ON StockAdjustment(warehouse_id);
CREATE INDEX idx_adjustment_product ON StockAdjustment(product_id);
CREATE INDEX idx_adjustment_status ON StockAdjustment(status);

-- StockTransfer indexes
CREATE INDEX idx_transfer_number ON StockTransfer(transfer_number);
CREATE INDEX idx_transfer_source ON StockTransfer(source_warehouse_id);
CREATE INDEX idx_transfer_dest ON StockTransfer(dest_warehouse_id);
CREATE INDEX idx_transfer_status ON StockTransfer(status);
CREATE INDEX idx_transfer_in_transit ON StockTransfer(status) WHERE status = 'in_transit';

-- StockAlert indexes
CREATE INDEX idx_alert_product ON StockAlert(product_id);
CREATE INDEX idx_alert_warehouse ON StockAlert(warehouse_id);
CREATE INDEX idx_alert_type ON StockAlert(alert_type);
CREATE INDEX idx_alert_status ON StockAlert(status) WHERE status = 'open';

-- StockBatch indexes
CREATE INDEX idx_batch_product ON StockBatch(product_id);
CREATE INDEX idx_batch_number ON StockBatch(batch_number);
CREATE INDEX idx_batch_expiry ON StockBatch(expiry_date);
CREATE INDEX idx_batch_status ON StockBatch(status);
```

### 4.4 Summary of Tables

| Table | Primary Key | Foreign Keys | Purpose | Estimated Rows |
|-------|-------------|--------------|---------|----------------|
| Warehouse | warehouse_id | User (manager) | Warehouse definitions | 10 |
| Location | location_id | Warehouse, Location (parent) | Location/bin hierarchy | 500 |
| WarehouseKeeper | keeper_id | Warehouse, User | Keeper assignments | 20 |
| StockItem | stock_id | Product, Warehouse, Location | Stock levels per product/warehouse | 50,000 |
| StockMovement | movement_id | Product, Warehouse, Location | Stock movement records | 500,000/year |
| StockCount | count_id | Warehouse, User | Stock count sessions | 200/year |
| StockCountItem | count_item_id | StockCount, StockItem | Count line items | 100,000/year |
| StockAdjustment | adjustment_id | Warehouse, Product, User | Stock adjustments | 2,000/year |
| StockTransfer | transfer_id | Warehouse (source/dest), User | Inter-warehouse transfers | 1,000/year |
| StockAlert | alert_id | Product, Warehouse, User | Stock level alerts | 5,000 |
| StockBatch | batch_id | Product, Warehouse | Batch/lot tracking | 10,000 |

---

## 5. API Specifications

### 5.1 Stock Item API

#### GET /api/v1/warehouse/stock-items

List current stock levels.

**Query Parameters**:
- `warehouse_id` (int): Filter by warehouse
- `product_id` (int): Filter by product
- `category_id` (int): Filter by category
- `status` (string): Filter by status (ok, low, over, zero)
- `search` (string): Search by product name/code
- `page` (int): Page number
- `per_page` (int): Items per page

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "stock_items": [
      {
        "stock_id": 5001,
        "product_id": 3001,
        "product_code": "PROD-001",
        "product_name": "Ao thun nam",
        "warehouse_id": 1,
        "warehouse_name": "Kho chinh",
        "location_id": 10,
        "location_code": "A-01-02",
        "quantity": 150,
        "reserved_quantity": 20,
        "available_quantity": 130,
        "cost_price": 500000,
        "total_value": 75000000,
        "min_level": 50,
        "max_level": 500,
        "status": "ok",
        "last_count_date": "2026-03-15"
      }
    ],
    "summary": {
      "total_items": 5000,
      "total_value": 640000000,
      "low_stock_count": 25,
      "zero_stock_count": 8,
      "over_stock_count": 5
    },
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total_pages": 250,
      "total_count": 5000
    }
  }
}
```

### 5.2 Stock Movement API

#### POST /api/v1/warehouse/movements

Record a stock movement.

**Request Body**:
```json
{
  "movement_type": "receipt",
  "product_id": 3001,
  "warehouse_id": 1,
  "location_id": 10,
  "quantity": 50,
  "unit_cost": 500000,
  "reference_type": "PO",
  "reference_id": 2201,
  "reference_number": "PO-2026-0022",
  "movement_date": "2026-04-14",
  "batch_number": "BATCH-2026-001",
  "notes": "Received from supplier IT Vina"
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "movement_id": 50001,
    "movement_number": "MV-2026-050001",
    "movement_type": "receipt",
    "product_id": 3001,
    "product_name": "Ao thun nam",
    "warehouse_id": 1,
    "warehouse_name": "Kho chinh",
    "location_id": 10,
    "location_code": "A-01-02",
    "quantity": 50,
    "unit_cost": 500000,
    "total_cost": 25000000,
    "reference_type": "PO",
    "reference_number": "PO-2026-0022",
    "movement_date": "2026-04-14",
    "batch_number": "BATCH-2026-001",
    "new_stock_quantity": 200,
    "created_at": "2026-04-14T10:00:00Z"
  }
}
```

### 5.3 Stock Count API

#### POST /api/v1/warehouse/stock-counts

Create a stock count session.

**Request Body**:
```json
{
  "warehouse_id": 1,
  "count_date": "2026-04-15",
  "count_type": "cycle",
  "scope_type": "category",
  "scope_value": [5, 10],
  "notes": "Monthly cycle count for electronics and office supplies"
}
```

**Response** (201 Created):
```json
{
  "success": true,
  "data": {
    "count_id": 201,
    "count_number": "SC-2026-0201",
    "warehouse_id": 1,
    "warehouse_name": "Kho chinh",
    "count_date": "2026-04-15",
    "count_type": "cycle",
    "scope_type": "category",
    "status": "in_progress",
    "total_items": 250,
    "counted_items": 0,
    "items_with_variance": 0,
    "items": [
      {
        "count_item_id": 50001,
        "stock_id": 5001,
        "product_name": "Ao thun nam",
        "location_code": "A-01-02",
        "system_quantity": 150,
        "status": "pending"
      }
    ],
    "created_at": "2026-04-14T09:00:00Z"
  }
}
```

#### PUT /api/v1/warehouse/stock-counts/{count_id}/items/{item_id}

Update counted quantity for a stock count item.

**Request Body**:
```json
{
  "counted_quantity": 148,
  "notes": "Found 148 pieces, 2 pieces missing"
}
```

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "count_item_id": 50001,
    "system_quantity": 150,
    "counted_quantity": 148,
    "variance": -2,
    "variance_pct": -1.33,
    "status": "variance_ok",
    "notes": "Found 148 pieces, 2 pieces missing",
    "counted_at": "2026-04-15T10:30:00Z"
  }
}
```

### 5.4 Stock Alert API

#### GET /api/v1/warehouse/stock-alerts

List active stock alerts.

**Response** (200 OK):
```json
{
  "success": true,
  "data": {
    "alerts": [
      {
        "alert_id": 101,
        "product_id": 3001,
        "product_name": "Ao thun nam",
        "warehouse_id": 1,
        "warehouse_name": "Kho chinh",
        "alert_type": "low_stock",
        "current_quantity": 30,
        "min_level": 50,
        "max_level": 500,
        "triggered_date": "2026-04-13T08:00:00Z",
        "status": "open"
      }
    ],
    "summary": {
      "total_alerts": 38,
      "low_stock": 25,
      "zero_stock": 8,
      "over_stock": 5,
      "open": 30,
      "acknowledged": 8
    }
  }
}
```

### 5.5 Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Stock movement validation failed",
    "details": [
      {
        "field": "quantity",
        "message": "Insufficient stock. Available: 130, Requested: 150"
      }
    ],
    "request_id": "req-wh123"
  }
}
```

### 5.6 API Summary Table

| Endpoint | Method | Auth Required | Rate Limit | Description |
|----------|--------|---------------|------------|-------------|
| /api/v1/warehouse/warehouses | GET | Yes | 200/min | List warehouses |
| /api/v1/warehouse/locations | GET | Yes | 200/min | List locations |
| /api/v1/warehouse/stock-items | GET | Yes | 200/min | List stock levels |
| /api/v1/warehouse/movements | POST | Yes | 100/min | Record movement |
| /api/v1/warehouse/movements | GET | Yes | 200/min | List movements |
| /api/v1/warehouse/adjustments | POST | Yes | 30/min | Create adjustment |
| /api/v1/warehouse/adjustments | GET | Yes | 200/min | List adjustments |
| /api/v1/warehouse/transfers | POST | Yes | 30/min | Create transfer |
| /api/v1/warehouse/transfers/{id}/issue | POST | Yes | 30/min | Issue transfer |
| /api/v1/warehouse/transfers/{id}/receive | POST | Yes | 30/min | Receive transfer |
| /api/v1/warehouse/stock-counts | POST | Yes | 20/min | Create count session |
| /api/v1/warehouse/stock-counts | GET | Yes | 200/min | List count sessions |
| /api/v1/warehouse/stock-counts/{id}/items/{item_id} | PUT | Yes | 100/min | Update count item |
| /api/v1/warehouse/stock-counts/{id}/approve | POST | Yes | 20/min | Approve count |
| /api/v1/warehouse/stock-alerts | GET | Yes | 100/min | List stock alerts |
| /api/v1/warehouse/stock-alerts/{id}/resolve | POST | Yes | 50/min | Resolve alert |
| /api/v1/warehouse/keepers | POST | Yes | 20/min | Assign keeper |
| /api/v1/warehouse/dashboard | GET | Yes | 100/min | Dashboard summary |
| /api/v1/warehouse/reports/valuation | GET | Yes | 30/min | Inventory valuation |

---

## 6. Permissions Matrix

### 6.1 Role Definitions

| Role | Code | Description | Department |
|------|------|-------------|------------|
| Warehouse Manager | WH_MGR | Full warehouse control | Warehouse |
| Warehouse Keeper | WH_KEEPER | Stock operations | Warehouse |
| Inventory Controller | INV_CTRL | Stock optimization | Warehouse |
| Stock Counter | WH_COUNTER | Physical counting | Warehouse |
| Finance Officer | FIN_OFFICER | Valuation tracking | Finance |
| Director | DIRECTOR | Strategic oversight | Executive |

### 6.2 CRUD Permissions

| Resource | WH_MGR | WH_KEEPER | INV_CTRL | WH_COUNTER | FIN_OFFICER | DIRECTOR |
|----------|--------|-----------|----------|------------|-------------|----------|
| Warehouse | CRUD | R | R | R | R | R |
| Location | CRUD | R | R | R | R | R |
| StockItem | CRUD | R | CRUD | R | R | R |
| StockMovement | CRUD | CRUD | R | R | R | R |
| StockAdjustment | CRUD + Approve | Create | R | R | R | R |
| StockTransfer | CRUD | CRUD | R | R | R | R |
| StockCount | CRUD + Approve | R | CRUD | CRUD | R | R |
| StockAlert | R | R | CRUD | R | R | R |
| WarehouseKeeper | CRUD | R (own) | R | R | R | R |

### 6.3 Row-Level Security

| Resource | Rule | Description |
|----------|------|-------------|
| StockItem | Keepers see only assigned warehouses | `warehouse_id IN (user.warehouses) OR user.role IN ('WH_MGR', 'INV_CTRL', 'DIRECTOR')` |
| StockMovement | Keepers see only assigned warehouses | Same as StockItem |
| StockCount | Counters see assigned count sessions | `warehouse_id IN (user.warehouses) OR user.role IN ('WH_MGR', 'INV_CTRL')` |

---

## 7. State Machine

### 7.1 Stock Movement State Diagram

```
                    +---------+
                    |  DRAFT  |
                    +----+----+
                         |
                   (Confirm)
                         v
                    +---------+
                    |POSTED   |
                    +----+----+
                         |
                   (Reversal if needed)
                         v
                    +---------+
                    |REVERSED |
                    +---------+
```

### 7.2 Stock Count State Diagram

```
                    +-------------+
                    | IN_PROGRESS |
                    +------+------+
                           |
                     (All counted)
                           v
                    +-------------+
                    |  COMPLETED  |
                    +------+------+
                           |
                     (Approved)
                           v
                    +-------------+
                    |   APPROVED  |
                    +------+------+
                           |
                     (Adjusted)
                           v
                    +-------------+
                    |  ADJUSTED   |
                    +-------------+
```

### 7.3 State Transition Tables

#### Stock Count Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| IN_PROGRESS | COMPLETED | All items counted | Stock Counter | All items have counted_quantity |
| COMPLETED | APPROVED | Approve count | Warehouse Manager | Variances reviewed |
| APPROVED | ADJUSTED | Adjust stock | System | Auto-adjust on approval |
| IN_PROGRESS | IN_PROGRESS | Add more counts | Stock Counter | Session open |
| COMPLETED | IN_PROGRESS | Reopen for re-count | Warehouse Manager | Variances need recount |

#### Stock Transfer Transitions

| From State | To State | Trigger | Required Role | Conditions |
|-----------|----------|---------|---------------|------------|
| DRAFT | ISSUED | Issue from source | Source Keeper | Stock available at source |
| ISSUED | IN_TRANSIT | Goods in transit | System | Issue confirmed |
| IN_TRANSIT | RECEIVED | Receive at destination | Dest Keeper | Goods received |
| RECEIVED | COMPLETED | Auto-complete | System | Receipt confirmed |
| DRAFT | CANCELLED | Cancel | Creator | Before issue |
| IN_TRANSIT | CANCELLED | Return to source | Both Keepers | Goods returned |

### 7.4 Approval Workflow

#### Stock Adjustment Approval

```
Keeper records adjustment
     |
     v
Check amount vs threshold
     /         \
  <=500K      >500K
     |          |
     v          v
  Auto      Manager
  Post      Approval
     |       /    \
     v   Approve  Reject
  Adjusted   |      |
             v      v
          Adjusted  Returned
```

---

## 8. Reports

### 8.1 Report Inventory

| Report | Description | Dimensions | Metrics | Filters | Chart Type |
|--------|-------------|------------|---------|---------|------------|
| Inventory Valuation | Stock value by product/warehouse | Product, warehouse, category | Quantity, cost, total value | Date, warehouse | Table |
| Stock Movement Summary | Movement activity analysis | Movement type, product, period | Movement count, total quantity, value | Period, type | Bar chart |
| Stock Count Variance | Count variance analysis | Warehouse, product, count session | Variance amount, variance %, count accuracy | Period, warehouse | Heat map |
| Stock Alert Report | Active and resolved alerts | Alert type, product, warehouse | Alert count, resolution rate | Period, type | Table |
| Stock Turnover | Inventory turnover analysis | Product, category, period | Turnover ratio, days of supply | Period, category | Line chart |
| Warehouse Utilization | Space and capacity usage | Warehouse, location | Utilization %, available capacity | Date, warehouse | Gauge chart |
| Transfer Summary | Inter-warehouse transfer analysis | Source, destination, period | Transfer count, quantity, value | Period | Sankey diagram |
| ABC Analysis | Product classification by value | Product, category | Value %, quantity %, cumulative % | Date | Pareto chart |

### 8.2 Report Summary Table

| Report ID | Report Name | Schedule | Auto-Generate | Distribution | Retention |
|-----------|-------------|----------|---------------|--------------|-----------|
| RPT-WH-001 | Inventory Valuation | Monthly | Yes | Finance, Warehouse Manager, Director | 5 years |
| RPT-WH-002 | Stock Movement Summary | Weekly | Yes | Warehouse Manager, Keeper | 2 years |
| RPT-WH-003 | Stock Count Variance | Per Count | Yes | Warehouse Manager | 3 years |
| RPT-WH-004 | Stock Alert Report | Daily | Yes | Inventory Controller, Manager | 1 year |
| RPT-WH-005 | Stock Turnover | Monthly | Yes | Warehouse Manager, Director | 3 years |
| RPT-WH-006 | Warehouse Utilization | Monthly | Yes | Warehouse Manager | 2 years |
| RPT-WH-007 | Transfer Summary | Monthly | Yes | Warehouse Manager | 2 years |
| RPT-WH-008 | ABC Analysis | Quarterly | Yes | Inventory Controller, Manager | 3 years |

---

## 9. Integration Points

### 9.1 External API Integrations

| Integration | Provider | Protocol | Direction | Frequency | Data Exchanged |
|------------|----------|----------|-----------|-----------|----------------|
| Barcode Scanner | Hardware | USB/Bluetooth | Inbound | Real-time | Barcode scan data |
| RFID System | Hardware | TCP/IP | Inbound | Real-time | RFID tag readings |
| Weighing Scale | Hardware | Serial/USB | Inbound | Real-time | Weight measurements |
| WMS Integration | External WMS | HTTPS/API | Bidirectional | Real-time | Stock data sync |
| Logistics Provider | Shipping company | HTTPS/API | Outbound | On shipment | Shipment creation, tracking |

### 9.2 Internal Module Integrations

| Source Module | Target | Data Flow | Trigger | Frequency |
|---------------|--------|-----------|---------|-----------|
| M09 Inventory | M04 Accounting | Stock movement -> GL entries | Movement posted | Real-time |
| M07 Purchasing | M09 Inventory | PO receipt -> Stock increase | Goods received | Real-time |
| M08 Sales | M09 Inventory | Sale order -> Stock deduction | Order confirmed | Real-time |
| M09 Inventory | M07 Purchasing | Low stock -> Auto PR | Stock below minimum | Real-time |
| M09 Inventory | M08 Sales | Stock level -> Product availability | Stock change | Real-time |
| M10 Asset Mgmt | M09 Inventory | Equipment -> Warehouse location | Asset assigned | Real-time |

### 9.3 Webhook Events

| Event | Trigger | Payload | Subscribers |
|-------|---------|---------|-------------|
| stock.movement_posted | Stock movement posted | movement_id, type, product, quantity, warehouse | Accounting, Sales, Purchasing |
| stock.low_stock | Stock below minimum | product_id, warehouse, current_qty, min_level | Inventory Controller, Purchasing |
| stock.zero_stock | Stock reached zero | product_id, warehouse, last_movement | Inventory Controller, Manager |
| stock.count_completed | Stock count completed | count_id, warehouse, items_counted, variances | Warehouse Manager |
| stock.adjustment_approved | Stock adjustment approved | adjustment_id, product, quantity, cost_impact | Accounting, Manager |
| transfer.received | Transfer received | transfer_id, source, destination, items | Source Keeper, Accounting |

---

## 10. Technical Notes

### 10.1 Performance Requirements

| Metric | Target | Measurement | Notes |
|--------|--------|-------------|-------|
| Stock Level Query | < 300ms | P95 latency | Single product/warehouse |
| Stock List (paginated) | < 800ms | P95 latency | 20 items per page |
| Movement Record | < 200ms | P95 latency | Single movement with stock update |
| Stock Count Save | < 500ms | P95 latency | Single item count |
| Bulk Movement Import | 1000 in < 30s | Throughput | With validation |
| Valuation Report | < 5s | P95 latency | Full warehouse valuation |
| Concurrent Users | 50+ | Simultaneous | Warehouse operations |

### 10.2 Caching Strategy

| Cache Type | Data | TTL | Invalidation |
|------------|------|-----|--------------|
| Redis | Stock levels (current) | 1 minute | On movement posted |
| Redis | Warehouse list | 1 hour | On warehouse change |
| Redis | Location tree | 1 hour | On location change |
| Redis | Alert count | 5 minutes | On alert change |
| Application | Product catalog | 15 minutes | On product update |
| Browser | Warehouse picker | 1 hour | On warehouse change |

### 10.3 Key Files and Components

| File/Component | Path | Purpose |
|----------------|------|---------|
| Stock Service | `src/modules/warehouse/services/stock.service.ts` | Stock level management |
| Movement Service | `src/modules/warehouse/services/movement.service.ts` | Movement recording and posting |
| Count Service | `src/modules/warehouse/services/count.service.ts` | Stock count sessions |
| Transfer Service | `src/modules/warehouse/services/transfer.service.ts` | Inter-warehouse transfers |
| Adjustment Service | `src/modules/warehouse/services/adjustment.service.ts` | Stock adjustments |
| Alert Service | `src/modules/warehouse/services/alert.service.ts` | Stock alert generation |
| Valuation Service | `src/modules/warehouse/services/valuation.service.ts` | Inventory valuation |
| Location Service | `src/modules/warehouse/services/location.service.ts` | Location management |
| Report Generator | `src/modules/warehouse/services/report-generator.service.ts` | Warehouse reports |
| Barcode Service | `src/modules/warehouse/services/barcode.service.ts` | Barcode processing |

### 10.4 Localization

| Aspect | Configuration | Notes |
|--------|---------------|-------|
| Language | Vietnamese (primary), English | All labels, messages |
| Currency | VND | All monetary values |
| Date Format | DD/MM/YYYY | Vietnamese standard |
| Number Format | 1.234.567.890 (dot separator) | Vietnamese standard |
| Unit of Measure | Vietnamese | cai, hop, chiec, kg, lit |
| Warehouse Names | Vietnamese | Per company convention |

### 10.5 Mobile Support

| Feature | Mobile Support | Notes |
|---------|----------------|-------|
| Stock Lookup | Responsive | Search and view stock levels |
| Movement Recording | Responsive | Record receipts and issues |
| Stock Counting | Touch-optimized | Barcode scan + quantity entry |
| Transfer Management | Responsive | Issue and receive transfers |
| Alert Viewing | Responsive | View and acknowledge alerts |
| Barcode Scanning | Camera integration | Scan product barcodes |
| Dashboard | Responsive | View KPIs and charts |

### 10.6 Security Considerations

| Aspect | Implementation |
|--------|----------------|
| Stock Movement Audit | Immutable log of all stock movements |
| Adjustment Approval | Manager approval required above threshold |
| Count Integrity | Blind count option to prevent bias |
| Keeper Accountability | Each keeper responsible for assigned warehouse |
| Negative Stock Prevention | System-enforced constraint |
| Data Integrity | Transaction-based stock updates |
| Access Control | Warehouse-level row security |

### 10.7 Database Considerations

- Stock quantity stored as DECIMAL(14,4) for precision
- All stock movements in same transaction as stock level update
- Materialized views for stock levels (refreshed on movement)
- Partition StockMovement table by month
- Archive closed stock count sessions after 1 year
- FIFO cost layers stored in separate table for detailed valuation
- Unique constraint on StockItem(product_id, warehouse_id)

---

*Document Version: 1.0*
*Last Updated: 2026-04-14*
*Author: ERP Design Team*
*Review Status: Draft*
