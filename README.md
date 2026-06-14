# Keycloak OIDC RBAC Lab

## Overview

This project demonstrates authentication and Role-Based Access Control (RBAC) using Keycloak and OpenID Connect (OIDC). Users authenticate through Keycloak and receive JSON Web Tokens (JWTs) containing role information that can be used by applications for authorization decisions.

---

## Technologies Used

* Keycloak
* OpenID Connect (OIDC)
* JSON Web Tokens (JWT)
* Docker
* Docker Compose
* Git
* GitHub
* PowerShell

---

## Project Objectives

The goals of this lab were to:

1. Deploy Keycloak using Docker.
2. Create a custom realm named **identity-lab**.
3. Configure users and roles.
4. Create an OpenID Connect client.
5. Authenticate users and obtain JWT access tokens.
6. Decode and inspect JWT payloads.
7. Demonstrate Role-Based Access Control using Keycloak roles.

---

## Environment Setup

### Start Keycloak

```bash
docker compose up -d
```

Access Keycloak at:

```text
http://localhost:8080
```

---

## Realm Configuration

Realm Name:

```text
identity-lab
```

This realm contains the users, roles, and client used throughout the lab.

---

## Users

### Alice

| Property   | Value                                         |
| ---------- | --------------------------------------------- |
| Username   | alice                                         |
| First Name | Alice                                         |
| Last Name  | Smith                                         |
| Email      | [alice@example.com](mailto:alice@example.com) |

Assigned Roles:

```text
user
```

---

### Bob

| Property   | Value                                     |
| ---------- | ----------------------------------------- |
| Username   | bob                                       |
| First Name | Bob                                       |
| Last Name  | Jones                                     |
| Email      | [bob@example.com](mailto:bob@example.com) |

Assigned Roles:

```text
manager
```

---

## Roles

The following realm roles were created:

| Role    | Purpose                                  |
| ------- | ---------------------------------------- |
| user    | Standard user access                     |
| manager | Elevated access for management functions |

---

## Client Configuration

Client ID:

```text
demo-app
```

Protocol:

```text
OpenID Connect
```

The client was configured to allow direct access grants for testing authentication and token generation.

---

## OpenID Connect Discovery

OIDC metadata endpoint:

```text
http://localhost:8080/realms/identity-lab/.well-known/openid-configuration
```

This endpoint provides information about:

* Authorization endpoint
* Token endpoint
* UserInfo endpoint
* JWKS endpoint
* Supported scopes
* Supported grant types

---

## Authentication Testing

Users authenticated using the Resource Owner Password Credentials grant for lab testing purposes.

Example request:

```powershell
$body = @{
    client_id = "demo-app"
    username = "alice"
    password = "Password123!"
    grant_type = "password"
    scope = "openid profile email"
}

$response = Invoke-RestMethod `
    -Method Post `
    -Uri "http://localhost:8080/realms/identity-lab/protocol/openid-connect/token" `
    -ContentType "application/x-www-form-urlencoded" `
    -Body $body
```

Successful authentication returns:

* Access Token
* Refresh Token
* ID Token

---

## JWT Validation

JWT access tokens were decoded to verify role assignments.

### Alice Token

Decoded payload contained:

```json
{
  "preferred_username": "alice",
  "realm_access": {
    "roles": [
      "user"
    ]
  }
}
```

### Bob Token

Decoded payload contained:

```json
{
  "preferred_username": "bob",
  "realm_access": {
    "roles": [
      "manager"
    ]
  }
}
```

These claims demonstrate successful implementation of Role-Based Access Control.

---

## Screenshots

The following screenshots document the lab:

### 1. Realm Configuration

* identity-lab realm

### 2. Client Configuration

* demo-app OpenID Connect client

### 3. Alice Role Mapping

* Alice assigned the user role

### 4. Bob Role Mapping

* Bob assigned the manager role

### 5. Alice JWT Payload

* Decoded token showing user role

### 6. Bob JWT Payload

* Decoded token showing manager role

### 7. OpenID Configuration Endpoint

* OIDC discovery document

### 8. GitHub Repository

* Project source code and documentation

---

## Repository Structure

```text
keycloak-oidc-rbac-lab/
│
├── docker-compose.yml
├── README.md
├── screenshots/
│   ├── realm.png
│   ├── client.png
│   ├── alice-role.png
│   ├── bob-role.png
│   ├── alice-token.png
│   ├── bob-token.png
│   ├── oidc-config.png
│   └── github-repo.png
│
└── docs/
```

---

## Conclusion

This lab successfully demonstrated:

* Keycloak deployment with Docker
* OpenID Connect authentication
* JWT token issuance and validation
* User and role management
* Role-Based Access Control (RBAC)
* GitHub project documentation and version control

The resulting configuration provides a foundation for integrating Keycloak authentication and authorization into modern web applications.
