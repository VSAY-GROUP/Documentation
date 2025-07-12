---
sidebar_label: 'Login Screen'
sidebar_position: 1
---

# 📄 Login Screen Documentation

## ✅ Overview
This document describes the architecture, flow, and functionality of the unified **Login Module** used across all products in your ecosystem.  
The module supports multi-tenant access with **Tenant Selection**, credential-based login, and Single Sign-On (SSO) with Microsoft and Google.

---

## 🗂️ Pages

The login module consists of **two main pages**:

---

### 1️⃣ Change Tenant Page

**Purpose**
- Allows the user to select or switch the tenant (organization/environment).
- Ensures that the user is authenticated within the correct tenant context.

**How it works**
- On the **first visit**, the user must choose their tenant.
- On **subsequent visits**, the tenant value is **persisted in local storage**, so the user bypasses this page and directly lands on the **Login Page**.
- Users can **manually switch tenants** using the **"Change Tenant"** option on the Login Page.

**Data Storage**
- **LocalStorage Key:** `tenantId`
- Value is used in API requests to authenticate against the correct tenant environment.

---

### 2️⃣ Login Screen

**Purpose**
Allows users to authenticate using:
- Username / Email & Password
- Social login (Microsoft, Google)
- Tenant switching

**Fields & Actions**

| Field                | Description                                |
|----------------------|--------------------------------------------|
| **Username / Email** | Required input for user identification     |
| **Password**         | Required input for authentication          |
| **Forgot Password**  | Link to reset the password                 |
| **Login with Microsoft** | Sign in using Microsoft SSO           |
| **Login with Google**    | Sign in using Google SSO              |
| **Change Tenant**        | Navigate back to the Tenant Selection page |

**Remember Tenant**
- When login is successful, the selected tenant remains stored in local storage.
- On logout, you can clear the tenant ID if you want to force tenant re-selection.

---

## ⚙️ Technical Details

### 🗂️ LocalStorage Keys

| Key         | Description                      |
|-------------|----------------------------------|
| `tenantId`  | Stores the selected tenant       |
| `authToken` | Stores the user’s auth token/session |

### 🔗 API Calls

- **POST /login**  
  Request: `{ username/email, password, tenantId }`  
  Response: `{ authToken, userInfo }`

- **POST /login/sso**  
  Microsoft & Google OAuth flows redirect back with an auth token.

---

## 🗺️ Architecture Diagram

```plaintext
+-------------------------+
|      Client Browser     |
|-------------------------|
| 1. Change Tenant Page   |
|    [tenantId stored]    |
|             |           |
|             v           |
| 2. Login Page           |
| - Username / Email      |
| - Password              |
| - Forgot Password       |
| - Microsoft Login       |
| - Google Login          |
| - Change Tenant         |
+-------------------------+
            |
            v
+-------------------------+
|      Auth Server/API    |
|-------------------------|
| - Validate credentials  |
| - Verify tenantId       |
| - Issue JWT/Session     |
| - Handle SSO providers  |
+-------------------------+
            |
            v
+-------------------------+
|    Local Storage        |
|-------------------------|
| tenantId                |
| authToken               |
+-------------------------+
