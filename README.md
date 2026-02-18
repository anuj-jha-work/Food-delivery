# 🍕 Xwiggy - Food Delivery Platform

A full-stack, production-ready food delivery application built with **Angular 8** and **Spring Boot**, featuring seamless user authentication, intelligent cart management, and a merchant dashboard for restaurant owners to manage their inventory in real-time.

---

## ✨ Key Features

- **🔐 Advanced User Authentication**: Secure login and registration system with encrypted password management using AES encryption
- **🛒 Intelligent Cart Management**: Add, update quantities, and manage items in real-time with instant price calculations
- **📋 Dynamic Menu System**: Browse and search through restaurant menus with rich product images and pricing
- **👨‍💼 Merchant Dashboard**: Complete restaurant management portal for merchants to add new food items, upload images, and manage inventory
- **📦 Order Management**: Seamless checkout process with order confirmation and tracking
- **✉️ Contact System**: Direct communication channel for customer feedback and support
- **🔄 Real-Time Synchronization**: Instant updates between frontend and backend using RESTful APIs
- **🎨 Responsive UI**: Mobile-friendly design using Bootstrap 4 and Angular Material components
- **🔒 Role-Based Access Control**: Separate user interfaces for customers and merchant partners

---

## 🛠️ Tech Stack

### Frontend
- **Angular 8** - Modern component-based architecture with RxJS for reactive programming
- **TypeScript** - Type-safe development with OOP principles
- **Bootstrap 4** - Responsive and elegant UI components
- **Font Awesome 4.7** - Comprehensive icon library for rich visual elements
- **RxJS 6.4** - Reactive extensions for asynchronous operations
- **Angular HTTP Client** - Seamless API communication

### Backend
- **Spring Boot 2.1.6** - Lightweight, production-ready framework
- **Spring Data JPA** - Powerful ORM with Hibernate for database operations
- **Spring MVC** - RESTful API design and controller layer
- **MySQL 10.4** - Robust relational database with InnoDB engine
- **Maven** - Build automation and dependency management
- **JAXB API** - XML parsing and serialization support

### Database & Infrastructure
- **MySQL** - Reliable ACID-compliant database with full-text search capabilities
- **InnoDB** - Transaction support and data integrity
- **RESTful Architecture** - Standard HTTP methods for CRUD operations
- **CORS** - Cross-Origin Resource Sharing for secure API communication

---

## 🏗️ System Architecture

The application follows a **three-tier distributed architecture** with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────────┐
│                    CLIENT LAYER (Browser)                        │
│              Angular 8 SPA with TypeScript Components            │
│         Bootstrap UI + Responsive Design + Real-Time Updates     │
└────────────────────┬────────────────────────────────────────────┘
                     │ (JSON/HTTPS)
                     ↓
┌─────────────────────────────────────────────────────────────────┐
│                   REST API LAYER (Spring Boot)                   │
│     Controllers → Services → DAOs (Data Access Objects)          │
│         Cross-Origin Request Handling + Input Validation         │
└────────────────────┬────────────────────────────────────────────┘
                     │ (JDBC/SQL)
                     ↓
┌─────────────────────────────────────────────────────────────────┐
│                  DATA LAYER (MySQL Database)                      │
│        Normalized Schema with Relational Constraints             │
│              Transaction Management + Data Integrity             │
└─────────────────────────────────────────────────────────────────┘
```

**Communication Flow:**
1. User interacts with Angular components in the browser
2. Angular HTTP Client makes asynchronous REST API calls to Spring Boot backend
3. Spring Boot controllers receive requests and route them to appropriate services
4. Services execute business logic and interact with DAOs
5. DAOs use Spring Data JPA/Hibernate for ORM operations
6. Data is persisted in MySQL with transaction support
7. Responses are serialized as JSON and sent back to the frontend
8. RxJS observables handle asynchronous data streams and UI updates

---

## 📊 Database Schema

The application uses a normalized relational database design with the following core entities:

### Entity Relationship Overview

| Table | Purpose | Key Fields | Relationships |
|-------|---------|-----------|---------------|
| **user** | User accounts and authentication | username (PK), password, firstname, lastname, email, address, phone, merchant | Links to cart and orders |
| **food** | Menu items managed by restaurants | id (PK), item, price, quantity, url, formID, cartID | References cart items |
| **cart** | Shopping cart state | quantity1-6 | Stores user's selected items and quantities |
| **contact** | Customer inquiries | contact_id (PK), name, email, message, timestamp | One-way relationship for feedback |

### Table Definitions

```sql
-- Users Table (Authentication & Profile)
CREATE TABLE user (
  username VARCHAR(45) PRIMARY KEY,
  password VARCHAR(45) NOT NULL,
  firstname VARCHAR(45) NOT NULL,
  lastname VARCHAR(45),
  email VARCHAR(45),
  address VARCHAR(45) NOT NULL,
  phone INT NOT NULL,
  merchant TINYINT NOT NULL  -- Flag: 0 = Customer, 1 = Merchant
);

-- Food Items Table (Menu Management)
CREATE TABLE food (
  id VARCHAR(45) PRIMARY KEY,
  item VARCHAR(45) NOT NULL,
  price INT NOT NULL,
  quantity INT,
  url VARCHAR(120),  -- Image URL
  formID VARCHAR(50),
  cartID VARCHAR(45)
);

-- Cart Table (Order Management)
CREATE TABLE cart (
  quantity1 INT DEFAULT 0,
  quantity2 INT DEFAULT 0,
  quantity3 INT DEFAULT 0,
  quantity4 INT DEFAULT 0,
  quantity5 INT DEFAULT 0,
  quantity6 INT DEFAULT 0
);

-- Contact Form Table (Customer Support)
CREATE TABLE contact (
  contact_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) NOT NULL,
  message TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🔌 API Endpoints Reference

Complete list of RESTful API endpoints for frontend integration:

| HTTP Method | Endpoint | Request Body | Response | Purpose |
|-------------|----------|--------------|----------|---------|
| **POST** | `/login` | `{username, password}` | `User object` | Authenticate user credentials and return user profile |
| **POST** | `/register` | `{username, password, firstname, lastname, email, address, phone, merchant}` | `Boolean` | Create new user account with encrypted password |
| **POST** | `/checkUserName` | `{username}` | `Boolean` | Validate username availability during registration |
| **GET** | `/menu` | - | `Food[] array` | Retrieve complete food menu with prices and images |
| **POST** | `/cart` | `NewCart[] array` | `Integer (total)` | Calculate order total and store cart state |
| **POST** | `/addToCart` | `NewCart[] array` | `NewCart[] array` | Update item quantities in active cart |
| **POST** | `/addNewItem` | `FormData: file + newFoodItem JSON` | `Boolean` | Add new menu item with image file upload (Merchant only) |
| **POST** | `/addNewItemUrl` | `{item, price, url}` | `Boolean` | Add new menu item using image URL (Merchant only) |
| **POST** | `/checkItemId` | `itemId string` | `Boolean` | Verify item ID uniqueness before creation |
| **POST** | `/contact` | `{name, email, message}` | `Boolean` | Submit customer feedback and inquiries |

### Example API Calls

**Login Request:**
```json
POST /login
Content-Type: application/json

{
  "username": "john_doe",
  "password": "SecurePass123"
}

Response (200 OK):
{
  "username": "john_doe",
  "firstname": "John",
  "lastname": "Doe",
  "email": "john@example.com",
  "address": "123 Main St",
  "phone": 9876543210,
  "merchant": false,
  "password": null
}
```

**Add Item to Cart Request:**
```json
POST /addToCart
Content-Type: application/json

[
  {"id": "abc", "quantity": 2},
  {"id": "bcd", "quantity": 1}
]

Response (200 OK):
[
  {"id": "abc", "quantity": 2},
  {"id": "bcd", "quantity": 1}
]
```

---

## 🚀 Local Setup & Installation

### Prerequisites

Before starting, ensure you have the following installed:

- **Java Development Kit (JDK) 8 or higher** - [Download here](https://www.oracle.com/java/technologies/downloads/)
- **Node.js 12+ and npm** - [Download here](https://nodejs.org/)
- **MySQL Server 5.7 or higher** - [Download here](https://dev.mysql.com/downloads/mysql/)
- **Git** - [Download here](https://git-scm.com/)
- **Maven 3.6+** - [Download via package manager or here](https://maven.apache.org/download.cgi)
- **Angular CLI 8** - To be installed via npm in next steps

### Step 1: Clone the Repository

```bash
# Clone the repository
git clone https://github.com/anuj-jha-work/Food-delivery.git

# Navigate to project directory
cd Food-delivery/food-order-Angular8-SpringBoot-MVC-JPA-MYSQL/Xwiggy-Angular8-SpringBoot-MVC-JPA-MYSQL
```

### Step 2: Setup MySQL Database

```bash
# Open MySQL command line
mysql -u root -p

# Create the database
CREATE DATABASE myusers;
USE myusers;
```

Now import the SQL schema:

```bash
# From the terminal, run the SQL script
mysql -u root -p myusers < "database sql file/myusers.sql"
```

Or manually execute the SQL file contents through MySQL Workbench if preferred.

### Step 3: Configure Spring Boot Backend

Navigate to the backend configuration file and update database credentials:

**File:** `xwiggy-back/src/main/resources/application.properties`

```properties
# Database Configuration (CRITICAL - Update these values)
spring.datasource.url=jdbc:mysql://localhost:3306/myusers?useSSL=false
spring.datasource.username=root
spring.datasource.password=<YOUR_MYSQL_PASSWORD>

# Hibernate Configuration
spring.jpa.hibernate.ddl-auto=none

# Encryption Key (Keep consistent for secure operations)
AES.Key=Bar12345Bar12345

# File Storage Path (For uploaded images)
fileStorage=/assets
```

**Important Notes:**
- Replace `<YOUR_MYSQL_PASSWORD>` with your actual MySQL root password
- If using a different MySQL user, update the `spring.datasource.username`
- Ensure MySQL server is running before starting the backend
- The `ddl-auto=none` setting prevents automatic schema creation; database must exist

### Step 4: Build and Run Spring Boot Backend

```bash
# Navigate to backend directory
cd xwiggy-back

# Clean previous builds
mvn clean

# Build the Spring Boot application
mvn build

# Run the Spring Boot application
mvn spring-boot:run

# The backend will start on http://localhost:8080
# Verify with: curl http://localhost:8080/menu
```

**Backend Running Indicators:**
- Terminal shows: `Started XwiggyApplication in X seconds`
- No error messages in console
- Tomcat server listens on port 8080

### Step 5: Setup and Run Angular Frontend

```bash
# Navigate to frontend directory
cd ../xwiggy-app

# Install all npm dependencies (First time only, takes ~2-3 minutes)
npm install

# Start Angular development server
npm start

# The application will automatically open in your default browser
# Frontend available at: http://localhost:4200
```

**Frontend Running Indicators:**
- Terminal shows: `✔ Compiled successfully`
- Browser opens to `http://localhost:4200`
- Hot reload is active for development

### Step 6: Access the Application

Open your browser and navigate to:

```
http://localhost:4200
```

### Testing the Application

**Create a Test User Account:**
1. Click on the **Register** button
2. Fill in the form with test credentials:
   - Username: `testuser123`
   - Password: `Password@123`
   - First Name: `Test`
   - Last Name: `User`
   - Email: `test@example.com`
   - Address: `123 Test Street`
   - Phone: `9876543210`
3. Uncheck the "Register as Merchant" checkbox for customer account
4. Click **Submit**

**Test Customer Flow:**
1. Login with created credentials
2. Browse the menu on the home page
3. Add items to cart
4. Proceed to checkout
5. Complete the order

**Test Merchant Flow:**
1. Register as a merchant (check "Register as Merchant" checkbox)
2. Login with merchant credentials
3. Navigate to Merchant Dashboard
4. Add new menu items with images or URLs
5. Manage food inventory

---

## 📁 Project Structure

```
Xwiggy-Angular8-SpringBoot-MVC-JPA-MYSQL/
│
├── xwiggy-app/                          # Angular Frontend
│   ├── src/
│   │   ├── app/
│   │   │   ├── login/                   # User login component
│   │   │   ├── register/                # User registration component
│   │   │   ├── menu/                    # Menu display component
│   │   │   ├── checkout/                # Order checkout component
│   │   │   ├── cart/                    # Shopping cart component
│   │   │   ├── home/                    # Landing page component
│   │   │   ├── merchant-menu/           # Merchant menu management
│   │   │   ├── merchant-welcome/        # Merchant dashboard
│   │   │   ├── contact-us/              # Contact form component
│   │   │   ├── success/                 # Order success page
│   │   │   ├── app.component.ts         # Root component
│   │   │   ├── app.module.ts            # Module configuration
│   │   │   ├── app-routing.module.ts    # Route definitions
│   │   │   └── menu-service.service.ts  # HTTP service for API calls
│   │   ├── assets/                      # Static images and resources
│   │   ├── environments/                # Environment configs
│   │   ├── styles.css                   # Global styles
│   │   └── index.html                   # Main HTML file
│   ├── package.json                     # npm dependencies and scripts
│   ├── angular.json                     # Angular CLI configuration
│   └── tsconfig.json                    # TypeScript configuration
│
├── xwiggy-back/                         # Spring Boot Backend
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/xwiggy/food/
│   │   │   │   ├── controller/          # REST API Controllers
│   │   │   │   │   ├── LoginController.java
│   │   │   │   │   ├── RegistrationController.java
│   │   │   │   │   ├── FoodController.java
│   │   │   │   │   ├── CartController.java
│   │   │   │   │   └── ContactController.java
│   │   │   │   ├── model/               # JPA Entity Classes
│   │   │   │   │   ├── User.java
│   │   │   │   │   ├── Food.java
│   │   │   │   │   ├── Cart.java
│   │   │   │   │   └── Contact.java
│   │   │   │   ├── dao/                 # Data Access Layer (DAOs)
│   │   │   │   │   ├── UserDaoImpl.java
│   │   │   │   │   ├── FoodDaoImpl.java
│   │   │   │   │   └── CartDaoImpl.java
│   │   │   │   ├── utility/             # Helper Classes
│   │   │   │   │   └── StrongAES.java   # Encryption utility
│   │   │   │   └── XwiggyApplication.java
│   │   │   ├── resources/
│   │   │   │   ├── application.properties  # Database & server config
│   │   │   │   └── static/              # Static web content
│   │   │   └── dbScripts/               # Database scripts
│   │   └── test/                        # JUnit test cases
│   ├── pom.xml                          # Maven configuration & dependencies
│   └── mvnw                             # Maven wrapper script
│
├── database sql file/
│   └── myusers.sql                      # Complete database schema
│
└── README.md                            # Project documentation
```

---

## 🔐 Security Features

- **Password Encryption**: AES-256 encryption for secure password storage
- **CORS Protection**: Cross-Origin Resource Sharing configured to prevent unauthorized API access
- **SQL Injection Prevention**: Parameterized queries through Spring Data JPA
- **Input Validation**: Server-side validation for all user inputs
- **Session Management**: Secure user session handling with logout functionality
- **HTTPS Ready**: Application architecture supports SSL/TLS deployment

---

## 🧪 Testing

### Frontend Testing
```bash
# Run Angular unit tests
npm test

# Run e2e tests
npm run e2e
```

### Backend Testing
```bash
# Run JUnit test suite
mvn test

# Access test reports
# Backend: target/surefire-reports/
```

---

## 🚢 Deployment

### Production Build - Frontend
```bash
cd xwiggy-app
ng build --prod
# Generates optimized files in dist/xwiggy-app
```

### Production Build - Backend
```bash
cd xwiggy-back
mvn clean package -DskipTests
# Generates executable JAR in target/xwiggy-0.0.1-SNAPSHOT.jar
```

### Run Production JAR
```bash
java -jar target/xwiggy-0.0.1-SNAPSHOT.jar
```

---

## 📚 Learning Resources

- **Spring Boot Docs**: https://spring.io/projects/spring-boot
- **Angular Official Guide**: https://angular.io/docs
- **MySQL Reference**: https://dev.mysql.com/doc/
- **RESTful API Design**: https://restfulapi.net/
- **JPA Hibernate**: https://hibernate.org/orm/

---

## 🤝 Contributing

Contributions are welcome! Please follow the existing code structure and commit guidelines.

---

## 📝 License

This project is open source and available under the MIT License.

---

## 👨‍💻 Author

**Developed by:** Anuj Jha  
**Repository**: [anuj-jha-work/Food-delivery](https://github.com/anuj-jha-work/Food-delivery)

---

## 📞 Support & Contact

For questions, issues, or suggestions:
- Open an issue on GitHub
- Use the in-app Contact Us feature
- Email: anuj.jha@example.com

---

**Last Updated:** February 2026  
**Version:** 1.0.0  
**Status:** ✅ Production Ready
