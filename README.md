<a name="top"></a>
<div align="center">

# 🍽️ Restaurant Management System  

**Modern Restaurant Management System**  
A powerful backend with Go and modern frontend technologies.

</div>

<div align="center" style="display: flex; flex-direction: column; gap: 10px;">

  <!-- Download Button -->
  <a href="https://github.com/deinrepo/restaurant-system/releases">
    <img src="https://img.shields.io/badge/Download-App-FF5722?style=for-the-badge&logo=github" alt="Download App">
  </a>

  <!-- Demo Button -->
  <a href="https://demo.restaurant-system.de">
    <img src="https://img.shields.io/badge/View-Demo-4CAF50?style=for-the-badge&logo=google-chrome" alt="View Demo">
  </a>

  <!-- Report Bug Button -->
  <a href="https://github.com/deinrepo/restaurant-system/issues">
    <img src="https://img.shields.io/badge/Report-Bug-red?style=for-the-badge&logo=github" alt="Report Bug">
  </a>

  <!-- Support Button -->
  <a href="https://t.me/deinsupport">
    <img src="https://img.shields.io/badge/Get-Support-blue?style=for-the-badge&logo=telegram" alt="Get Support">
  </a>
</div>


<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZDA3OWRjYzY1NzY5ODA4YjQwZDhkYjRkY2EwYzQyYzFmZDBiMDA3YiZlcD12MV9pbnRlcm5hbF9naWZzX2dpZklkJmN0PWc/fwpE4Y7bsJpQgKwofy/giphy.gif" width="500">

# ✨ Features
- Modern. Practical. Optimized for every use case.

- 🆓 Open Source & Freedom
100% free and open source under the MIT license – fully customizable and extensible.

- 👥 Role-Based Workflows
🧑🍳 Waitstaff Dashboard:

- Create new orders, add menu items, and save special requests.

- Prioritize urgent orders with a single click.

- Send orders directly to the kitchen.

- 👨🍳 Kitchen Dashboard:

- Update order status (e.g., "In Preparation" → "Ready").

- Sort orders by table, status, or elapsed time.

- Filter active/completed orders.

- ⚡ Real-Time Interaction
- Instant status updates between waitstaff and kitchen (UI refresh).

- Simulated live view of all active orders.

- 🛠️ Technical Strengths
- Cross-Platform: Runs on Windows, macOS*, and Linux* via .NET 6+ (*with compatibility layer).

- Modular Architecture: Clear separation of logic and UI (WPF/XAML).

- Error-Resistant UI: Warnings for missing selections, consistent data handling.

- 📦 Data Management
  Local storage for orders, special requests, and prioritizations (in-memory).

- Easy integration with databases (SQL Server, SQLite).

- 🚀 Dev-Friendly
- Clean code structure using Dictionary and HashSet for fast iteration.

- UI components like ListView, TextBox, and Button for intuitive usability.


 # 📋 Requirements
- [x] **SQL Server Management Studio (SSMS)** ([Download](https://aka.ms/ssmsfullsetup))  
- [x] **VPN-Connection** to Hochschule Flensburg (IP: `vpn.hs-flensburg.de`)  
- [x] University Login (Username  + Password)  
            <div align ="right">                                                      [↑Back to Top](#top) </div>
# 🚀 Database installation  
## 1. Datebase set up  
1. Connect to the SSMS with:  
   - **Server name**: `goliath.wi.fh-flensburg.de`  
   - **Authentication**: `SQL Server Authentication`  
   - **Login/Password**: Your university credential



2. **Execute the SQL-script ** (ersetze `Your_Username`):  
```sql
-- Datenbank erstellen
CREATE DATABASE RestaurantDB_Your_Username;
GO

USE RestaurantDB_Your_Username;
GO

-- Tabellen anlegen
CREATE TABLE Users (
    UserID NVARCHAR(50) PRIMARY KEY,
    Username NVARCHAR(50) NOT NULL UNIQUE,
    Password NVARCHAR(100) NOT NULL,
    Role NVARCHAR(20) NOT NULL,
    Name NVARCHAR(100),
    IsActive BIT DEFAULT 1
);

CREATE TABLE MenuItems (
    ItemID NVARCHAR(50) PRIMARY KEY,
    Name NVARCHAR(100) NOT NULL,
    Description NVARCHAR(500),
    Price DECIMAL(10,2) NOT NULL,
    Category NVARCHAR(50),
    IsAvailable BIT DEFAULT 1
);

CREATE TABLE Orders (
    OrderID NVARCHAR(50) PRIMARY KEY,
    TableNumber NVARCHAR(10),
    OrderDate DATETIME DEFAULT GETDATE(),
    Status NVARCHAR(20),
    SpecialRequests NVARCHAR(500),
    TotalAmount DECIMAL(10,2),
    IsPriority BIT DEFAULT 0,
    WaiterID NVARCHAR(50),
    FOREIGN KEY (WaiterID) REFERENCES Users(UserID)
);

CREATE TABLE OrderItems (
    OrderID NVARCHAR(50),
    MenuItemID NVARCHAR(50),
    Quantity INT NOT NULL,
    UnitPrice DECIMAL(10,2) NOT NULL,
    SpecialInstructions NVARCHAR(200),
    PRIMARY KEY (OrderID, MenuItemID),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (MenuItemID) REFERENCES MenuItems(ItemID)
);

CREATE TABLE Payments (
    PaymentID NVARCHAR(50) PRIMARY KEY,
    OrderID NVARCHAR(50) UNIQUE,
    Amount DECIMAL(10,2) NOT NULL,
    PaymentType NVARCHAR(20),
    PaymentDate DATETIME DEFAULT GETDATE(),
    Status NVARCHAR(20),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

CREATE TABLE Tables (
    TableNumber NVARCHAR(10) PRIMARY KEY,
    Capacity INT NOT NULL,
    Status NVARCHAR(20)
);
GO

-- Values hinzufügen
INSERT INTO Users (UserID, Username, Password, Role, Name, IsActive)
VALUES 
('1', 'waiter1', 'password', 'Waitstaff', 'John Doe', 1),
('2', 'kitchen1', 'password', 'Kitchen', 'Jane Smith', 1),
('3', 'manager1', 'password', 'Manager', 'Mike Johnson', 1);

INSERT INTO MenuItems (ItemID, Name, Description, Price, Category, IsAvailable)
VALUES 
('1', 'Spaghetti Bolognese', 'Klassische italienische Pasta', 12.99, 'Hauptgericht', 1),
('2', 'Caesar Salad', 'Frischer Römersalat mit Caesar-Dressing', 8.99, 'Vorspeise', 1),
('3', 'Tiramisu', 'Italienische Kaffee-Nachspeise', 6.99, 'Dessert', 1);  
```

  
  

# Configure Database Connection 🔧
To allow the app to access your database, update the connection string in the app.config file:
1. Open app.config in the project folder.
2. Locate the <connectionStrings> section:
``` csharp
<connectionStrings>
  <add name="RestaurantDBConnectionString" 
       connectionString="Server=goliath.wi.fh-flensburg.de;Database=RestaurantDB_LUPR7441;Trusted_Connection=True;" 
       providerName="System.Data.SqlClient" />
</connectionStrings>
```
3. Change the database name: Replace RestaurantDB_LUPR7441 with your own name, e.g., RestaurantDB_YOUR_USERNAME.
4. Save the file and restart the app.






# ❓ FAQ
### 1. Why can't I connect to the database?
- **Check VPN:** Ensure you're connected to the university VPN (vpn.hs-flensburg.de).
- **Verify Credentials:** Double-check your SQL Server username and password.
- **Database Name:** Make sure the database name in app.config matches the one in SSMS.

### 2. How do I add new menu items?
- Currently, menu items are added via SQL scripts. Use the following example:
  ```
  INSERT INTO MenuItems (ItemID, Name, Description, Price, Category, IsAvailable)VALUES ('4', 'Margherita Pizza', 'Classic Italian pizza', 10.99, 'Main Course', 1);
  ```
### 3. How do I prioritize an order?
- In the **Waitstaff Dashboard:**
```
1. Select an order from the list.
2. Click the "Prioritize Order" button.
3. The order will be marked as "Prioritized" in the kitchen view.
```
### 4. How do I update the order status in the kitchen?
- In the **Kitchen Dashboard:**
```
1. Select an order from the list.
2. Click "Mark as Ready" when the order is complete.
3. The order will move to the "Completed Orders" list.
```
### 5. Where can I get support?
- E- Mail us at: restaurantapp@nichtverfügbar.de


# 📜 License Information 📜
This project is licensed under the **MIT License.**

What does this mean?
- You are free to use, modify, and distribute the code for personal or commercial purposes.
- Attribution is required (include the original license in your project).
- The authors are not liable for any damages caused by the use of this software.

For the complete license text, see [LICENSE](LICENSE)
    <div align ="right"> [↑Back to Top](#top) </div>
