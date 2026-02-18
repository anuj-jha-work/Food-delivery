# 🍕 Food Delivery Application

A full-stack **Food Delivery Platform** built with **Spring Boot** and **MySQL** that enables customers to browse restaurants, place orders, and track deliveries in real-time. This enterprise-grade application features secure user authentication, robust cart management, and a comprehensive admin dashboard for restaurant owners.

---

## ✨ Key Features

- **User Authentication & Authorization**: Secure JWT-based authentication with role-based access control (Customer, Restaurant Owner, Admin)
- **Restaurant Management**: Browse restaurants by cuisine type, ratings, and location with advanced filtering
- **Menu & Cart Management**: Add items to cart, modify quantities, and apply promotional discount codes
- **Order Processing & Tracking**: Real-time order status tracking from placement to delivery
- **Payment Gateway Integration**: Mock payment processing with support for multiple payment methods
- **Restaurant Dashboard**: Comprehensive dashboard for restaurant owners to manage menus, view orders, and update order status
- **Order History**: Complete order history with detailed receipts and reorder functionality
- **Search & Filter**: Advanced search capabilities for restaurants and menu items
- **Responsive Design**: Mobile-first responsive UI for seamless experience across all devices

---

## 🛠️ Tech Stack

### Backend
- **Framework**: Spring Boot 2.7+ (Java 11+)
- **Security**: Spring Security with JWT Authentication
- **Database**: MySQL 8.0
- **ORM**: Spring Data JPA / Hibernate
- **API**: RESTful APIs with JSON responses
- **Build Tool**: Maven
- **Validation**: Bean Validation (JSR-380)

### Frontend
- **Framework**: HTML5, CSS3, JavaScript (ES6+)
- **Styling**: Bootstrap 5 / Tailwind CSS
- **AJAX**: Fetch API for asynchronous requests
- *(Can be replaced with React, Angular, or Vue.js)*

### Database
- **RDBMS**: MySQL 8.0
- **Connection Pooling**: HikariCP

### Development Tools
- **IDE**: IntelliJ IDEA / Eclipse / VS Code
- **API Testing**: Postman
- **Version Control**: Git & GitHub

---

## 🏗️ System Architecture

The application follows a **three-tier architecture** pattern:

1. **Presentation Layer (Frontend)**: 
   - User interface built with HTML/CSS/JavaScript
   - Communicates with backend via RESTful API calls using Fetch/Axios
   - Handles user interactions and displays dynamic content

2. **Business Logic Layer (Backend)**:
   - Spring Boot application exposes RESTful endpoints
   - Handles authentication, authorization, and business rules
   - Processes incoming requests and returns JSON responses
   - Implements service layer for business logic separation

3. **Data Access Layer (Database)**:
   - MySQL database stores all persistent data
   - Spring Data JPA repositories provide abstraction over database operations
   - Entity relationships managed through JPA annotations

**Communication Flow**:
```
Frontend (UI) → HTTP/HTTPS → Spring Boot REST Controllers → Service Layer → Repository Layer → MySQL Database
```

**Authentication Flow**:
- User credentials sent to `/api/auth/login`
- Backend validates and returns JWT token
- Frontend stores token and includes it in Authorization header for subsequent requests
- Backend validates JWT on each protected endpoint request

---

## 🗄️ Database Schema

### Main Tables

1. **users**
   - `id` (Primary Key)
   - `username`, `email`, `password` (hashed)
   - `phone_number`, `address`
   - `role` (CUSTOMER, RESTAURANT_OWNER, ADMIN)
   - `created_at`, `updated_at`

2. **restaurants**
   - `id` (Primary Key)
   - `name`, `description`, `cuisine_type`
   - `address`, `phone_number`
   - `rating`, `delivery_time`
   - `owner_id` (Foreign Key → users)
   - `is_active`, `created_at`

3. **menu_items**
   - `id` (Primary Key)
   - `restaurant_id` (Foreign Key → restaurants)
   - `name`, `description`, `price`
   - `category` (STARTER, MAIN_COURSE, DESSERT, BEVERAGE)
   - `is_vegetarian`, `is_available`
   - `image_url`, `created_at`

4. **orders**
   - `id` (Primary Key)
   - `user_id` (Foreign Key → users)
   - `restaurant_id` (Foreign Key → restaurants)
   - `total_amount`, `delivery_address`
   - `status` (PLACED, CONFIRMED, PREPARING, OUT_FOR_DELIVERY, DELIVERED, CANCELLED)
   - `payment_method`, `payment_status`
   - `order_time`, `delivery_time`

5. **order_items**
   - `id` (Primary Key)
   - `order_id` (Foreign Key → orders)
   - `menu_item_id` (Foreign Key → menu_items)
   - `quantity`, `price`
   - `special_instructions`

6. **cart**
   - `id` (Primary Key)
   - `user_id` (Foreign Key → users)
   - `menu_item_id` (Foreign Key → menu_items)
   - `quantity`
   - `added_at`

---

## 📡 API Endpoints

### Authentication APIs

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| POST | `/api/auth/register` | Register new user | `{username, email, password, role}` |
| POST | `/api/auth/login` | User login | `{email, password}` |
| POST | `/api/auth/logout` | User logout | JWT Token in header |

### Restaurant APIs

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| GET | `/api/restaurants` | Get all restaurants | Not Required |
| GET | `/api/restaurants/{id}` | Get restaurant by ID | Not Required |
| POST | `/api/restaurants` | Create new restaurant | Required (ADMIN) |
| PUT | `/api/restaurants/{id}` | Update restaurant | Required (OWNER) |
| DELETE | `/api/restaurants/{id}` | Delete restaurant | Required (ADMIN) |

### Menu APIs

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| GET | `/api/restaurants/{restaurantId}/menu` | Get menu items | Not Required |
| POST | `/api/restaurants/{restaurantId}/menu` | Add menu item | Required (OWNER) |
| PUT | `/api/menu/{id}` | Update menu item | Required (OWNER) |
| DELETE | `/api/menu/{id}` | Delete menu item | Required (OWNER) |

### Cart APIs

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| GET | `/api/cart` | Get user's cart | Required |
| POST | `/api/cart/add` | Add item to cart | Required |
| PUT | `/api/cart/update/{id}` | Update cart item quantity | Required |
| DELETE | `/api/cart/remove/{id}` | Remove item from cart | Required |
| DELETE | `/api/cart/clear` | Clear entire cart | Required |

### Order APIs

| Method | Endpoint | Description | Authentication |
|--------|----------|-------------|----------------|
| POST | `/api/orders` | Place new order | Required |
| GET | `/api/orders` | Get user's order history | Required |
| GET | `/api/orders/{id}` | Get order details | Required |
| PUT | `/api/orders/{id}/status` | Update order status | Required (OWNER) |
| DELETE | `/api/orders/{id}/cancel` | Cancel order | Required |

---

## 🚀 Local Setup & Installation

### Prerequisites

Before you begin, ensure you have the following installed:
- **Java JDK 11 or higher** ([Download](https://www.oracle.com/java/technologies/downloads/))
- **Maven 3.6+** ([Download](https://maven.apache.org/download.cgi))
- **MySQL 8.0+** ([Download](https://dev.mysql.com/downloads/))
- **Git** ([Download](https://git-scm.com/downloads))
- **Postman** (Optional, for API testing) ([Download](https://www.postman.com/downloads/))

### Step 1: Clone the Repository

```bash
git clone https://github.com/anuj-jha-work/Food-delivery.git
cd Food-delivery
```

### Step 2: Configure MySQL Database

1. Login to MySQL:
```bash
mysql -u root -p
```

2. Create a new database:
```sql
CREATE DATABASE food_delivery_db;
```

3. Create a database user (optional but recommended):
```sql
CREATE USER 'fooddelivery_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON food_delivery_db.* TO 'fooddelivery_user'@'localhost';
FLUSH PRIVILEGES;
```

### Step 3: Configure Application Properties

Navigate to `src/main/resources/application.properties` and update the following:

```properties
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/food_delivery_db?useSSL=false&serverTimezone=UTC
spring.datasource.username=fooddelivery_user
spring.datasource.password=your_password

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true

# Server Configuration
server.port=8080

# JWT Configuration (Add your secret key)
jwt.secret=YourSecretKeyForJWTTokenGenerationShouldBeLongAndSecure
jwt.expiration=86400000

# File Upload Configuration
spring.servlet.multipart.max-file-size=5MB
spring.servlet.multipart.max-request-size=5MB
```

### Step 4: Build the Project

```bash
mvn clean install
```

This command will:
- Download all dependencies
- Compile the source code
- Run tests (if any)
- Package the application into a JAR file

### Step 5: Run the Application

#### Option 1: Using Maven
```bash
mvn spring-boot:run
```

#### Option 2: Using Java JAR
```bash
java -jar target/food-delivery-0.0.1-SNAPSHOT.jar
```

### Step 6: Verify Installation

1. Check if the application is running:
   - Open your browser and navigate to: `http://localhost:8080`
   - Or use curl: `curl http://localhost:8080/api/restaurants`

2. Check application logs:
   - Look for `Started Application in X seconds` message
   - Verify database connection is successful

3. Test with Postman:
   - Import API endpoints
   - Test registration: `POST http://localhost:8080/api/auth/register`
   - Test login: `POST http://localhost:8080/api/auth/login`

### Step 7: Access the Application

- **Frontend URL**: `http://localhost:8080` (if serving static files)
- **API Base URL**: `http://localhost:8080/api`
- **H2 Console** (if using H2 for development): `http://localhost:8080/h2-console`

---

## 📝 Sample API Usage

### Register a New User
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "john_doe",
    "email": "john@example.com",
    "password": "securePassword123",
    "role": "CUSTOMER"
  }'
```

### Login
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "securePassword123"
  }'
```

### Get All Restaurants
```bash
curl -X GET http://localhost:8080/api/restaurants
```

### Place an Order (Authenticated)
```bash
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "restaurantId": 1,
    "deliveryAddress": "123 Main St",
    "paymentMethod": "CARD",
    "items": [
      {"menuItemId": 1, "quantity": 2},
      {"menuItemId": 3, "quantity": 1}
    ]
  }'
```

---

## 🧪 Testing

Run tests using Maven:
```bash
mvn test
```

Run specific test class:
```bash
mvn test -Dtest=RestaurantServiceTest
```

---

## 📦 Project Structure

```
Food-delivery/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/fooddelivery/
│   │   │       ├── controller/       # REST Controllers
│   │   │       ├── service/          # Business Logic
│   │   │       ├── repository/       # Data Access Layer
│   │   │       ├── model/            # Entity Classes
│   │   │       ├── dto/              # Data Transfer Objects
│   │   │       ├── config/           # Configuration Classes
│   │   │       ├── security/         # Security & JWT
│   │   │       ├── exception/        # Custom Exceptions
│   │   │       └── util/             # Utility Classes
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── static/               # Frontend files
│   │       └── templates/            # HTML templates (if using Thymeleaf)
│   └── test/
│       └── java/                     # Test classes
├── pom.xml                           # Maven dependencies
└── README.md
```

---

## 🔒 Security Features

- **Password Encryption**: BCrypt hashing algorithm
- **JWT Authentication**: Stateless token-based authentication
- **Role-Based Access Control**: Different permissions for Customers, Owners, and Admins
- **SQL Injection Prevention**: Parameterized queries via JPA
- **XSS Protection**: Input validation and sanitization
- **CORS Configuration**: Controlled cross-origin resource sharing

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👨‍💻 Author

**Anuj Jha**
- GitHub: [@anuj-jha-work](https://github.com/anuj-jha-work)

---

## 📧 Contact

For any queries or suggestions, please reach out:
- Email: your.email@example.com
- LinkedIn: [Your LinkedIn Profile](https://linkedin.com/in/yourprofile)

---

## 🙏 Acknowledgments

- Spring Boot Documentation
- MySQL Documentation
- Stack Overflow Community
- GitHub Community

---

**⭐ If you find this project useful, please consider giving it a star!**
