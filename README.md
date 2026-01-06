# 🚗 Vehicle Service Management System

A secure, scalable **full-stack web application** designed to digitize and automate operations of automobile service centers.  
Built using **ASP.NET Core Web API** and **Angular**, the system manages service requests, technician assignments, inventory, billing, payments, notifications, and reporting.

---

## 📌 Key Features

- 🔐 **Secure Authentication & Authorization**
  - JWT-based authentication
  - Role-Based Access Control (Admin, Service Manager, Technician, Customer)

- 🛠️ **Service Management**
  - Create and track vehicle service requests
  - Full service lifecycle: Requested → Assigned → In Progress → Completed → Closed

- 👨‍🔧 **Technician Management**
  - Technician assignment
  - Workload tracking and status updates

- 📦 **Inventory & Parts Management**
  - Spare parts tracking
  - Automatic stock deduction on usage
  - Low-stock monitoring

- 💳 **Billing & Payments**
  - Automatic invoice generation
  - Payment tracking and invoice status updates

- 📊 **Reports & Analytics**
  - Revenue reports
  - Technician workload reports
  - Monthly and vehicle service history reports
  - LINQ-based filtering and aggregation

- 🔔 **Notifications**
  - Service and billing status notifications

---

## 🏗️ System Architecture

- **Frontend:** Angular
- **Backend:** ASP.NET Core Web API
- **Database:** SQL Server
- **ORM:** Entity Framework Core
- **Querying:** LINQ
- **Security:** JWT + RBAC
- **Architecture Pattern:** Clean Architecture

---

## 👥 User Roles

- **Admin**
- **Service Manager**
- **Technician**
- **Customer**

Each role has controlled access to features based on RBAC policies.

---

## 📁 Project Structure
```
VehicleServiceManagementApp.zip
│
├── Backend/                    # ASP.NET Core Web API
│   ├── Controllers
│   ├── Application
│   ├── Domain
│   ├── Infrastructure
│   ├── Program.cs
│   └── appsettings.json
│
├── Frontend/                   # Angular Application
│   ├── src/
│   ├── angular.json
│   ├── package.json
│   └── node_modules/
│
├── Documents/                  # Functional Requirements, Architecture Docs
│
├── UI Screenshots/             # Application UI Workflow Screenshots
│
└── README.md
```

---

## ⚙️ Setup Instructions

### 🔹 Step 1: Extract the Project
> ⚠️ **Important:**  
If you downloaded the project as a ZIP file, **extract the ZIP file first** before proceeding.

---

### 🔹 Step 2: Backend Setup (ASP.NET Core API)

1. Open the `Backend` folder in **Visual Studio**
2. Update database connection string in `appsettings.json`
3. Restore NuGet packages
4. Apply migrations (if applicable)
5. Run the project

The API will start at: https://localhost:7029
Swagger UI: https://localhost:7029/swagger


---

### 🔹 Step 3: Frontend Setup (Angular)

1. Open the `Frontend` folder in **VS Code**
2. Install dependencies:
   ```bash
   npm install
   
Start Angular application
```
ng serve
```
Open browser
```
http://localhost:4200
```

