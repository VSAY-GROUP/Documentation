---
sidebar_label: 'Auth Architecture'
sidebar_position: 2
---

# 🔑 **VSAY GROUP - Unified Auth Architecture Documentation**

---

## ✅ **Purpose**

This document describes the **authentication and multi-tenancy architecture** for the VSAY GROUP ecosystem.  
It explains how **Tenants**, **Organizations**, and **Products** are structured under a single authentication module to ensure **secure and scalable access** across all services.

---

## 🗂️ **Key Concepts**

---

### 1️⃣ **Tenant**

- **Definition:**  
  A **Tenant** represents a top-level customer environment or business entity that subscribes to VSAY GROUP services.
- **Example:**  
  A large client company that uses your suite of products.
- **Responsibilities:**  
  - Stores tenant-specific configurations and settings.
  - Acts as a container for multiple **Organizations**.

---

### 2️⃣ **Organization**

- **Definition:**  
  An **Organization** sits **inside a Tenant** and represents a logical business unit, department, or subsidiary.
- **Example:**  
  Within a Tenant, you might have different organizations for:
  - R&D Department
  - Sales Team
  - Regional Offices
- **Responsibilities:**  
  - Contains users, roles, and permissions.
  - Manages access to specific **Products** for its members.

---

### 3️⃣ **Products**

- **Definition:**  
  The actual software solutions, services, or platforms provided by VSAY GROUP.
- **Examples:**  
  - AI Platform
  - Robotics Control System
  - ML Model Deployment Hub
  - Custom Engineering Software
- **Responsibilities:**  
  - Each Product integrates with the **Single Auth Service** for consistent user validation.
  - Access is granted based on the **Organization’s subscriptions** and user roles.

---

## 🔒 **Auth Flow**

### ✅ **How Authentication Works**

1. **Tenant Selection**
   - User chooses or is assigned a Tenant.
   - Tenant ID is stored in **local storage** for persistence.

2. **Organization Context**
   - After Tenant selection, the system loads available Organizations under that Tenant.
   - Users can belong to one or multiple Organizations.

3. **User Login**
   - User authenticates once using username/password or SSO (Microsoft/Google).
   - Auth Service verifies:
     - Tenant validity
     - Organization membership
     - Product permissions

4. **Product Access**
   - Auth token includes:
     - Tenant ID
     - Organization ID
     - User roles/permissions
   - Token is sent with API requests to any Product in the ecosystem.

---

## 🗺️ **Architecture Diagram**

```plaintext
+-----------------------------+
|         VSAY GROUP          |
+-----------------------------+
            |
            v
+-----------------------------+
|          Tenants            |  (Multiple)
+-----------------------------+
            |
            v
+-----------------------------+
|       Organizations         |  (Multiple per Tenant)
+-----------------------------+
            |
            v
+-----------------------------+
|         Users               |
|   - Roles & Permissions     |
+-----------------------------+
            |
            v
+-----------------------------+
|         Products            |  (Access controlled via Single Auth)
+-----------------------------+
