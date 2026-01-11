# GA SYSTEM - ROLE PERMISSIONS & ACCESS CONTROL

## 🔐 ROLE HIERARCHY
```
ADMIN (Highest Access)
  ↓
CS (Customer Service - Medium Access)  
  ↓
USER (Limited Access)
```

---

## 👑 **ADMIN ROLE**
**Department:** GA (General Affairs)
**Full Access to All Features**

### 📱 **Sidebar Menu:**
- ✅ Dashboard
- ✅ Requests
- ✅ Item Stock
- ✅ Calendar
- ✅ Account Management

### 🏠 **Dashboard Features:**
- ✅ Calendar widget
- ✅ Search item functionality
- ✅ Recent requests view
- ✅ Total pending requests counter

### 📋 **Requests Page:**
- ✅ View all requests from all departments
- ✅ Filter by date, item, department
- ✅ **APPROVE/REJECT requests** (Admin only)
- ✅ Add comments to requests
- ✅ Export to Excel
- ✅ Full request history access

### 📦 **Item Stock Page:**
- ✅ View all items and stock levels
- ✅ **Edit stock (Barang Masuk)** - Add new stock
- ✅ Create new items
- ✅ Stock reports with date filtering
- ✅ Export stock data to Excel
- ✅ View stock process history

### 📅 **Calendar Page:**
- ✅ **Create/Edit/Delete events** (Admin & CS only)
- ✅ View all calendar events
- ✅ Full calendar management

### 👥 **Account Management Page:**
- ✅ **Create new user accounts** (Admin only)
- ✅ **Edit user information** (Admin only)
- ✅ **Delete user accounts** (Admin only)
- ✅ **Reset user passwords** (Admin only)
- ✅ Manage CS and User accounts
- ✅ Assign departments to users

---

## 🎧 **CS (Customer Service) ROLE**
**Department:** WH (Warehouse)
**Medium Access - Support & Stock Management**

### 📱 **Sidebar Menu:**
- ✅ Dashboard
- ✅ Requests
- ✅ Item Stock
- ✅ Calendar
- ❌ Account Management (No Access)

### 🏠 **Dashboard Features:**
- ✅ Calendar widget
- ✅ Search item functionality
- ✅ Recent requests view
- ✅ **Total pending delivery counter** (CS specific)

### 📋 **Requests Page:**
- ✅ View all requests from all departments
- ✅ Filter by date, item, department
- ❌ Cannot approve/reject requests
- ✅ **Update delivery information** (CS only)
- ✅ **Update delivery status** (CS only)
- ✅ Export to Excel
- ✅ View request details

### 📦 **Item Stock Page:**
- ✅ View all items and stock levels
- ✅ **Edit stock (Barang Masuk)** - Add new stock
- ✅ Create new items
- ✅ Stock reports with date filtering
- ✅ Export stock data to Excel
- ✅ View stock process history

### 📅 **Calendar Page:**
- ✅ **Create/Edit/Delete events** (Admin & CS only)
- ✅ View all calendar events
- ✅ Calendar management

### 👥 **Account Management:**
- ❌ **No Access** - Cannot manage accounts

---

## 👤 **USER ROLE**
**Department:** Various (Production, QA/QC, Purchasing, etc.)
**Limited Access - Request Only**

### 📱 **Sidebar Menu:**
- ✅ Dashboard
- ✅ Requests
- ❌ Item Stock (No Access)
- ❌ Calendar (No Access)
- ❌ Account Management (No Access)

### 🏠 **Dashboard Features:**
- ✅ Search item functionality (view only)
- ✅ Recent requests view (own requests only)
- ✅ Total pending requests counter (own requests)
- ❌ No calendar widget

### 📋 **Requests Page:**
- ✅ **Create new requests** (User only)
- ✅ View own requests only
- ✅ Filter own requests by date, item
- ✅ Export own requests to Excel
- ❌ Cannot view other department requests
- ❌ Cannot approve/reject requests
- ❌ Cannot update delivery information

### 📦 **Item Stock Page:**
- ❌ **No Access** - Cannot view stock page

### 📅 **Calendar Page:**
- ❌ **No Access** - Cannot view calendar

### 👥 **Account Management:**
- ❌ **No Access** - Cannot manage accounts

---

## 🔒 **ACCESS CONTROL SUMMARY**

| Feature | Admin | CS | User |
|---------|-------|----|----- |
| **Dashboard** | ✅ Full | ✅ Full | ✅ Limited |
| **View All Requests** | ✅ Yes | ✅ Yes | ❌ Own Only |
| **Create Requests** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Approve/Reject Requests** | ✅ Yes | ❌ No | ❌ No |
| **Update Delivery** | ✅ Yes | ✅ Yes | ❌ No |
| **Stock Management** | ✅ Yes | ✅ Yes | ❌ No |
| **Calendar Management** | ✅ Yes | ✅ Yes | ❌ No |
| **Account Management** | ✅ Yes | ❌ No | ❌ No |
| **Export Data** | ✅ All Data | ✅ All Data | ✅ Own Data |

---

## 🏢 **DEPARTMENT ASSIGNMENTS**

### **Fixed Departments:**
- **Admin** → GA (General Affairs)
- **CS** → WH (Warehouse)

### **User Departments:**
- HRGA Legal
- HSE
- FAT (Finance Accounting Tax)
- Production
- QA/QC
- Purchasing
- PPIC Warehouse EXIM (Export Import)
- IT
- Sales
- Maintenance

---

## 🔐 **AUTHENTICATION & SECURITY**

### **Login Process:**
1. All roles use same login page
2. System automatically redirects based on role
3. JWT token contains role information
4. Frontend checks role before showing features

### **Route Protection:**
- `/dashboard` - All roles
- `/requests` - All roles (different data access)
- `/stock` - Admin & CS only
- `/calendar` - Admin & CS only
- `/accounts` - Admin only

### **API Endpoint Protection:**
- Authentication required for all API calls
- Role-based middleware checks permissions
- Users can only access their own data
- Admin has full access to all data
- CS has access to all requests and stock data

---

## 📝 **WORKFLOW EXAMPLES**

### **Request Approval Workflow:**
1. **User** creates request
2. **Admin** reviews and approves/rejects
3. **CS** updates delivery information
4. **User** receives notification of status

### **Stock Management Workflow:**
1. **Admin/CS** adds new stock (Barang Masuk)
2. **Admin/CS** processes stock out for approved requests
3. **Admin/CS** generates reports
4. **User** can search items but cannot see stock levels

### **Account Management Workflow:**
1. **Admin** creates new user accounts
2. **Admin** assigns departments and roles
3. **Admin** can reset passwords
4. **CS/User** cannot access account management