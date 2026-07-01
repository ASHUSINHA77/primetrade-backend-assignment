# Scalable REST API with Auth & Role-Based Access

A scalable, secure backend REST API featuring robust JWT authentication, Role-Based Access Control (RBAC), and a modular database schema, accompanied by a supportive frontend dashboard interface. Built as part of the backend engineering evaluation for Primetrade.ai.

## 🚀 Features

### Backend (Primary Focus)
- **User Authentication**: Secure registration and login with password hashing and robust JWT token issuance.
- **Role-Based Access Control (RBAC)**: Route protection differentiating privileges between standard `User` and `Admin` accounts.
- **Secondary Entity CRUD**: Complete, validated endpoints to Create, Read, Update, and Delete resources (e.g., Tasks/Products).
- **Production Standard Engineering**: Strict API versioning (`/api/v1/...`), robust global error handling, and tight input validation/sanitization.
- **API Documentation**: Interactive documentation ready via Postman/Swagger.

### Frontend (Supportive UI)
- Clean, responsive dashboard connecting seamlessly to the APIs.
- Functional states handling user registration, persistent login sessions (via secure token storage), dynamic CRUD interactions, and intuitive error/success message displays.

---

## 📁 Repository Structure

```text
├── backend/
│   ├── src/
│   │   ├── config/          # Database connection & JWT configurations
│   │   ├── controllers/     # Core business logic handlers (Auth & Entity CRUD)
│   │   ├── middleware/      # Auth verification & RBAC route guards
│   │   ├── models/          # Database entity schemas
│   │   ├── routes/          # Version-controlled API routing structures
│   │   └── utils/           # Input validation rules and password utilities
│   ├── package.json
│   └── .env.example
├── frontend/
│   ├── src/                 # Client dashboard codebase
│   └── package.json
├── api-docs/                # Postman Collection JSON / Swagger specifications
└── README.md                # Root project configuration documentation
