# ShopSmart BW — Android Business Management & POS App

**ShopSmart BW** is a complete, offline-first Android business management application specifically designed for small tuckshops, general dealers, and informal traders in **Botswana**.

Built with native **Kotlin**, **Jetpack Compose (Material 3)**, and **Room (SQLite)** local persistence.

---

## Key Features

1. **Botswana Pula (BWP / P) Financials & Dashboard**
   - Live Today's Sales Revenue (Cash + Credit recognized sales)
   - Real-time Cash Collected vs. Credit Sales
   - Cost of Goods Sold (COGS) & Gross Profit with margin %
   - Operational Expenses tracking
   - Net Operating Profit (`Gross Profit - Expenses`)
   - Total Outstanding Customer Debts (Accounts Receivable)
   - Total Stock Valuation at Cost
   - Date range filters: **Today, Yesterday, This Week, This Month, All Time**

2. **Fast POS Checkout**
   - Quick barcode/SKU & text search
   - Category filtering (Groceries, Beverages, Bakery, Dairy & Eggs, Cleaning, Confectionery, Airtime)
   - Real-time cart drawer with quantity stepper and discounts
   - Multi-payment support:
     - **Cash** (with tender calculator and change-due display)
     - **Orange Money**
     - **Smega (BTC)**
     - **MyZaka (Mascom)**
     - **Card / FNB eWallet**
     - **Credit Sale** (tied to customer account)
     - **Split Payments** (e.g. Cash + Mobile Money, or Cash + Credit)
   - Atomic database transactions preventing inconsistent stock or payment records

3. **WhatsApp Receipts & Customer Statements**
   - Professional receipt formatted with Shop Name, Plot/Location, Phone, Receipt #, Line Items, Totals, and Botswana thank-you message (*"Ke a leboga!"*)
   - Direct WhatsApp integration with pre-filled receipt (`api.whatsapp.com`) and system share sheet fallback
   - Clear UI indication distinguishing opening the share screen from message delivery

4. **Customer Credit & Debt Ledger**
   - Track customer names, phone numbers, and balances
   - Partial and full debt repayments with payment method logging
   - Validates that repayment does not exceed remaining debt
   - Instant **WhatsApp Payment Reminder** formatted in polite Batswana style

5. **Inventory & Stock Management**
   - Track cost price (buying) vs selling price and profit margins
   - Low-stock and out-of-stock badges and alerts
   - Restock workflow with quantity additions and invoice cost updates
   - Full chronological **Stock Movement Audit Trail** (Purchases, Sales, Adjustments, Returns)
   - Safe negative stock protection toggle

6. **Expenses & Financial Reports**
   - Record operating expenses: Rent, Electricity (BPC), Transport, Wages, Internet/Airtime, Repairs, Packaging, Misc
   - Reversal workflow with audit trail reasons
   - Financial report adhering to standard accounting principles:
     - Gross Profit = Sales Revenue - COGS
     - Net Operating Profit = Gross Profit - Expenses
     - Cash Flow calculated separately from profit (Stock purchases tracked in cash flow rather than deducted all at once from profit)
   - Best-selling products leaderboard
   - **One-tap CSV Exports** for Products, Sales, Customer Debts, and Expenses

7. **Local Backup & Restore**
   - 100% offline-first; no internet connection required for core shop operations
   - Export full business data to JSON backup
   - Restore database from JSON backup file
   - Pre-loaded with authentic Botswana tuckshop sample items (White Star Maize Meal, Tastic Rice, Sunfoil Cooking Oil, Stoney Ginger Beer, Chappies, Mascom/Orange Airtime vouchers)

---

## Architecture & Database Schema

The app follows **Clean Architecture & MVVM**:

- **Presentation Layer**: Jetpack Compose (M3) declarative UI, `ShopViewModel` with `StateFlow`.
- **Domain Layer**: Models (`CartItem`, `FinancialReportSummary`, `TopProductItem`), `DateFilterPeriod`, `CurrencyFormatter`, `WhatsAppHelper`, `CsvExporter`.
- **Data Layer**: Room SQLite Database (`AppDatabase`), DAOs (`ProductDao`, `CustomerDao`, `SaleDao`, `ExpenseDao`, `StockMovementDao`, `ShopSettingsDao`), `ShopRepository` with atomic `@Transaction` methods.

### Database Tables:
- `products`: id, name, skuOrBarcode, category, buyingPrice, sellingPrice, quantity, unit, minStockLevel, createdAt, updatedAt
- `customers`: id, name, phone, totalDebt, totalPaid, notes, createdAt
- `sales`: id, receiptNumber, subtotal, discountAmount, totalAmount, totalCostOfGoodsSold, amountPaid, creditAmount, paymentMethod, splitDetails, customerId, customerName, customerPhone, timestamp, status, notes
- `sale_items`: id, saleId (FK), productId, productName, quantity, unit, unitCostPrice, unitSellingPrice, subtotal
- `debt_payments`: id, customerId (FK), saleId, amount, paymentMethod, timestamp, notes
- `expenses`: id, category, amount, paymentMethod, description, supplierOrRecipient, date, isReversed, reversalReason
- `stock_movements`: id, productId, productName, type, quantityChange, previousQuantity, newQuantity, unitCost, notes, timestamp
- `shop_settings`: id, businessName, phone, address, currencySymbol, receiptFooter, lowStockThresholdDefault, pinCode, allowNegativeStock

---

## Build & Testing Instructions

### Run Automated Tests (Unit & Robolectric)
```bash
gradle :app:testDebugUnitTest
```

### Build Debug APK
```bash
gradle :app:assembleDebug
```
The output APK will be located at:
`app/build/outputs/apk/debug/app-debug.apk`

---

## License
Created for small businesses and informal traders in Botswana.
