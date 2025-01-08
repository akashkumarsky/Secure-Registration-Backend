 Secure Registration Backend
1. Description
This application is a Spring Boot-based project that enables user registration and login functionality with email validation. The primary purpose of the application is to provide users with the ability to register, log in, and authenticate with email verification. The application is built with Spring Boot, Spring Security, Spring Data JPA, and PostgreSQL, and it also uses email functionality for account confirmation.

1.1 Technologies Used
Backend: Spring Boot 3.4.1
Database: PostgreSQL (JPA)
Email Service: Spring Boot Email Integration
Security: Spring Security (for user authentication)
Logging: Spring Boot’s built-in logging mechanism
Other Libraries: Lombok (for reducing boilerplate code)

2. Project Setup
2.1 Prerequisites
JDK 21 or higher
Ensure Java Development Kit (JDK) 21 or higher is installed.

Database

PostgreSQL: Set up a PostgreSQL database. The application is configured to use PostgreSQL as its data source.
Maven

The project uses Maven for dependency management.
SMTP Server

The project is configured to use a local SMTP server for email sending. (Can be replaced with any SMTP provider for production)

3. Configuration
3.1 application.yml
This file contains configurations for the server, database, JPA, and mail.

yaml
Copy code
server:
  port: 9972
  error:
    include-message: always
    include-binding-errors: always

spring:
  datasource:
    username: postgres
    password: root
    url: jdbc:postgresql://localhost:5432/registration

  jpa:
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
        format_sql: true
    show-sql: true

  mail:
    host: localhost
    port: 1025
    username: root
    password: root
    properties:
      mail:
        smtp:
          ssl:
            trust: "*"
          auth: true
          starttls:
            enable: true
          connectiontimeout: 5000
          timeout: 3000
          writetimeout: 5000

3.2 Database Configuration
The application is configured to use a PostgreSQL database. Make sure to adjust the username, password, and url according to your local PostgreSQL setup.

4. Modules
4.1 EmailValidator
A class to validate email addresses using a regex pattern.

Location: com.sky.Register.loginlogout.registration.EmailValidator
Functionality: Validates the format of email addresses.
4.2 RegistrationController
Handles user registration, email confirmation, and retrieving user information based on the token.

Location: com.sky.Register.loginlogout.registration.RegistrationController
Endpoints:
POST /api/v1/registration: Registers a new user.
GET /api/v1/registration/confirm: Confirms the user's email.
GET /api/v1/registration/user: Retrieves a user by confirmation token.
4.3 RegistrationService
Contains the core business logic for user registration, token confirmation, and user retrieval.

Location: com.sky.Register.loginlogout.registration.RegistrationService
Key Methods:
register(RegistrationRequest request): Handles user registration and email sending.
confirmToken(String token): Confirms the user's email.
getUserByToken(String token): Retrieves the user associated with the token.
4.4 AppUserService
Responsible for handling operations related to AppUser entities.

Location: com.sky.Register.loginlogout.appuser.AppUserService
Functionality: Manages user signup, email activation, etc.
4.5 ConfirmationTokenService
Manages the creation and validation of confirmation tokens.

Location: com.sky.Register.loginlogout.registration.token.ConfirmationTokenService
Functionality: Handles the generation, expiration, and confirmation of tokens.
4.6 SecurityConfig
Configures security settings, including authentication and authorization.

Location: com.sky.Register.loginlogout.security.config.WebSecurityConfig
Key Features:
Authentication Manager: Manages authentication using AppUserService and BCryptPasswordEncoder.
Authorization Rules: Specifies routes that can be accessed without authentication (/api/v1/registration/**).
4.7 PasswordEncoder
Configures BCryptPasswordEncoder for secure password storage.

Location: com.sky.Register.loginlogout.security.PasswordEncoder
5. APIs
5.1 Registration Endpoint
URL: POST /api/v1/registration
Request Body:
json
Copy code
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "password123"
}
Response: Returns a confirmation token if the registration is successful.
5.2 Confirmation Endpoint
URL: GET /api/v1/registration/confirm
Parameters: token
Response: Confirms the user's email and activates their account.
5.3 Get User by Token
URL: GET /api/v1/registration/user
Parameters: token
Response: Returns user details associated with the token.

6. Security
6.1 Authentication
The application uses basic authentication with Spring Security. It supports user login and logout.

6.2 Password Storage
Passwords are stored securely using BCryptPasswordEncoder, which hashes passwords before storing them in the database.

6.3 Role-Based Access Control
The application uses role-based access control (RBAC) for authorization. By default, users are assigned the USER role upon registration.

7. Email Configuration
7.1 SMTP Setup
The project uses a local SMTP server running on port 1025 for sending registration confirmation emails. Modify the application.yml file for production SMTP services (e.g., Gmail, SendGrid).
Name-maildev
using Node.js
commend - 1 npm install -g maildev
2 maildev
3 copy MailDev webapp running at http://localhost:1080/ (web Address)
MailDev SMTP Server running at localhost:1025 (mail port)
  
