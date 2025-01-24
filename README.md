<a name="top"></a>
<div align="center">

# 🍽️ Restaurant Management System  

**Modernes Restaurant-Management-System**  
Ein leistungsstarkes Backend mit Go und modernen Frontend-Technologien.  

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




## ✨ Features
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
- [x] **VPN-Verbindung** zur Hochschule Flensburg (IP: `vpn.hs-flensburg.de`)  
- [x] Hochschul-Login (Benutzername + Passwort)  
            <div align ="right">                                                      [↑Back to Top](#top) </div>
## 🚀 Installation  
### 1. Datenbank einrichten  
1. Verbinde dich in SSMS mit:  
   - **Server name**: `goliath.wi.fh-flensburg.de`  
   - **Authentication**: `SQL Server Authentication`  
   - **Login/Passwort**: Deine Hochschuldaten  



2. **Führe dieses SQL-Skript aus** (ersetze `DEIN_BENUTZERNAME`):  
```sql
-- Datenbank erstellen
CREATE DATABASE RestaurantDB_DEIN_BENUTZERNAME;
GO

USE RestaurantDB_DEIN_BENUTZERNAME;
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


