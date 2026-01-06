# Vehicle Service Management API - Setup Instructions

## Prerequisites

Before setting up the project, ensure you have the following installed:

1. **.NET 8.0 SDK** or later
   - Download from: https://dotnet.microsoft.com/download/dotnet/8.0
   - Verify installation: `dotnet --version` (should show 8.0.x or later)

2. **SQL Server** (one of the following):
   - **SQL Server Express/Developer Edition** (recommended)
   - **SQL Server LocalDB** (included with Visual Studio)
   - **SQL Server** (full version)
   
   Verify SQL Server is running and accessible.

3. **Visual Studio 2022** or **Visual Studio Code** (optional, but recommended)
   - Visual Studio 2022 Community Edition (free): https://visualstudio.microsoft.com/
   - VS Code with C# extension: https://code.visualstudio.com/

## Project Structure

```
VehicleServiceApi/
├── Vsm.Api/              # Main API project (startup project)
├── Vsm.Application/       # Application services layer
├── Vsm.Domain/           # Domain entities and enums
├── Vsm.Infrastructure/   # Data access, migrations, identity
├── Vsm.Tests/            # Unit and integration tests
└── Vsm.sln               # Solution file
```

## Setup Steps

### Step 1: Clone/Extract the Project

1. Extract or clone the project to your local machine
2. Navigate to the project root directory:
   ```powershell
   cd "C:\path\to\VehicleServiceApi"
   ```

### Step 2: Restore NuGet Packages

Restore all NuGet packages for the solution:

```powershell
dotnet restore
```

This will download all required packages defined in the `.csproj` files.

### Step 3: Configure Database Connection

1. Open `Vsm.Api/appsettings.json` in a text editor
2. Update the connection string if needed. The default connection string uses LocalDB:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=VsmDb;Trusted_Connection=True;TrustServerCertificate=True;"
     }
   }
   ```

   **For SQL Server Express/Full SQL Server**, use:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=localhost;Database=VsmDb;Trusted_Connection=True;TrustServerCertificate=True;"
     }
   }
   ```

   **For SQL Server with username/password**, use:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Server=localhost;Database=VsmDb;User Id=your_username;Password=your_password;TrustServerCertificate=True;"
     }
   }
   ```

### Step 4: Create Database and Apply Migrations

**IMPORTANT:** This project already contains 18 migrations in `Vsm.Infrastructure/Migrations/`. You should **NOT** create a new initial migration unless you've deleted all existing migrations.

#### Normal Setup (Recommended)

Simply apply existing migrations to the database:

```powershell
dotnet ef database update --project Vsm.Infrastructure --startup-project Vsm.Api --context AppDbContext
```

This command will:
- Create the database if it doesn't exist
- Apply all 18 existing migrations in order:
  - InitIdentity
  - AddCustomerVehicle
  - UpdateVehicleSchema
  - AddServiceRequests
  - AddPartsInventory
  - AddBillingInvoices
  - AddNotifications
  - And other schema updates
- Set up all tables, indexes, and relationships

#### Option B: If Migrations Folder is Empty or Deleted (Only if needed)

**⚠️ WARNING:** Only use this if you've deleted all migrations and need to start fresh:

1. **Create Initial Migration:**
   ```powershell
   dotnet ef migrations add InitialCreate --project Vsm.Infrastructure --startup-project Vsm.Api --context AppDbContext
   ```

2. **Apply Migration to Database:**
   ```powershell
   dotnet ef database update --project Vsm.Infrastructure --startup-project Vsm.Api --context AppDbContext
   ```

### Step 5: Verify Database Creation

You can verify the database was created by:

1. **Using SQL Server Management Studio (SSMS):**
   - Connect to your SQL Server instance
   - Check if `VsmDb` database exists
   - Verify tables are created (AspNetUsers, Customers, Vehicles, ServiceRequests, etc.)

2. **Using Command Line:**
   ```powershell
   sqlcmd -S "(localdb)\MSSQLLocalDB" -Q "SELECT name FROM sys.databases WHERE name = 'VsmDb'"
   ```

### Step 6: Build the Solution

Build the entire solution to ensure everything compiles:

```powershell
dotnet build
```

You should see "Build succeeded" with no errors.

### Step 7: Run the Application

Run the API project:

```powershell
dotnet run --project Vsm.Api
```

Or if you're in the Vsm.Api directory:

```powershell
cd Vsm.Api
dotnet run
```

The application will start and you should see output like:
```
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://localhost:5001
```

### Step 8: Access Swagger UI

Once the application is running:

1. Open your browser
2. Navigate to: `https://localhost:5001/swagger` (or `http://localhost:5000/swagger` if HTTPS is disabled)
3. You should see the Swagger UI with all available API endpoints

### Step 9: Test the API

The application automatically seeds initial data in Development mode:

- **Default Users** (created by IdentityDbSeeder):
  - **Admin:**
    - Username: `admin` / Password: `Admin@123`
  - **Service Manager:**
    - Username: `manager` / Password: `Manager@123`
  - **Technicians:**
    - Username: `tech` / Password: `Tech@123`
    - Username: `tech1` / Password: `Tech@123`
    - Username: `tech2` / Password: `Tech@123`
  - **Customers:**
    - Username: `pratyush` / Password: `Customer@123`
    - Username: `kaustabh` / Password: `Customer@123`
    - Username: `aman` / Password: `Customer@123`
    - Username: `riya` / Password: `Customer@123`

  **Note:** Users are created with usernames (not email addresses). Use the username for login.

- **Sample Data** (created by DbSeeder):
  - Sample customers linked to customer users
  - Sample vehicles
  - Service categories (Oil Change, Brake Service, Engine Tune-up, AC Service, General Service)
  - Parts inventory (Engine Oil, Oil Filter, Brake Pads, Air Filter, Spark Plugs)

## Troubleshooting

### Issue: "Cannot open database" or "Login failed"

**Solution:**
- Verify SQL Server is running
- Check the connection string in `appsettings.json`
- Ensure the database server name is correct
- For LocalDB, try: `sqllocaldb start MSSQLLocalDB`

### Issue: "Migration already applied" or Migration conflicts

**Solution:**
- If you get migration conflicts, you can reset the database:
  ```powershell
  dotnet ef database drop --project Vsm.Infrastructure --startup-project Vsm.Api --context AppDbContext --force
  dotnet ef database update --project Vsm.Infrastructure --startup-project Vsm.Api --context AppDbContext
  ```

### Issue: "Column already exists" error

**Solution:**
- This usually means the database schema is out of sync
- Drop and recreate the database (see above)
- Or manually fix the migration if you understand the schema

### Issue: Entity Framework Tools not found

**Solution:**
- Install EF Core tools globally:
  ```powershell
  dotnet tool install --global dotnet-ef
  ```
- If already installed, update it:
  ```powershell
  dotnet tool update --global dotnet-ef
  ```

### Issue: Port already in use

**Solution:**
- Change the port in `Vsm.Api/Properties/launchSettings.json`
- Or stop the process using the port

## Running Tests

To run the unit and integration tests:

```powershell
dotnet test
```

Tests use an in-memory SQLite database, so no database setup is required for testing.

## Project Configuration

### JWT Settings

JWT token settings are in `Vsm.Api/appsettings.json`:

```json
{
  "Jwt": {
    "Key": "THIS_IS_DEMO_KEY_CHANGE_IT_1234567890",
    "Issuer": "VsmApi",
    "Audience": "VsmClient",
    "ExpireMinutes": 120
  }
}
```

**Note:** For production, change the JWT Key to a secure, randomly generated key.

### CORS Settings

CORS is configured to allow all origins in development. For production, update the CORS policy in `Program.cs`.

## Database Schema Overview

The application uses the following main entities:

- **AspNetUsers** - Identity users (Admin, ServiceManager, Technician, Customer)
- **Customers** - Customer information
- **Vehicles** - Vehicle information linked to customers
- **ServiceCategories** - Service types (Oil Change, Brake Service, etc.)
- **ServiceRequests** - Service requests from customers
- **Parts** - Inventory parts
- **ServicePartUsages** - Parts used in service requests
- **Invoices** - Billing invoices
- **InvoiceLines** - Invoice line items
- **Notifications** - User notifications

## API Endpoints

Key endpoints (see Swagger for complete list):

- **Auth:**
  - `POST /api/auth/register` - Register new user
  - `POST /api/auth/login` - Login and get JWT token

- **Customers:**
  - `GET /api/customers` - List customers
  - `POST /api/customers` - Create customer

- **Vehicles:**
  - `GET /api/vehicles` - List vehicles
  - `POST /api/vehicles` - Create vehicle

- **Service Requests:**
  - `GET /api/servicerequests` - List service requests
  - `POST /api/servicerequests` - Create service request

- **Billing:**
  - `POST /api/billing/generate-invoice` - Generate invoice
  - `POST /api/billing/pay-invoice` - Pay invoice

## Additional Notes

1. **Development vs Production:**
   - In Development mode, the app automatically seeds sample data
   - In Production, disable automatic seeding

2. **Migrations:**
   - All migrations are in `Vsm.Infrastructure/Migrations/`
   - The project currently has 18 migrations that should be applied in order
   - Never delete migration files unless you're resetting the entire database
   - Always test migrations on a copy of production data first
   - Current migrations include: InitIdentity, AddCustomerVehicle, AddServiceRequests, AddPartsInventory, AddBillingInvoices, AddNotifications, and various schema updates

3. **Logging:**
   - Logs are configured in `appsettings.json`
   - Adjust log levels as needed

## Support

If you encounter any issues during setup:

1. Check the error message carefully
2. Verify all prerequisites are installed
3. Ensure SQL Server is running
4. Check the connection string format
5. Review the Troubleshooting section above

## Quick Start Summary

For experienced developers, here's the quick setup:

```powershell
# 1. Restore packages
dotnet restore

# 2. Update database connection in appsettings.json if needed

# 3. Apply migrations
dotnet ef database update --project Vsm.Infrastructure --startup-project Vsm.Api --context AppDbContext

# 4. Run the application
dotnet run --project Vsm.Api

# 5. Open browser to https://localhost:5001/swagger
```

---



