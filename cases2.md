# End-to-End Testing Plan for MicroTask App

This document outlines the testing plan for all endpoints in the MicroTask App. It covers the entire lifecycle for each role: **Admin**, **Employer**, and **Worker**.

---

## **List of All Endpoints**

### **1. Authentication Endpoints**
| **Endpoint**                     | **Method** | **Access**      | **Description**                                                                 |
|----------------------------------|------------|-----------------|---------------------------------------------------------------------------------|
| `/api/auth/register`             | POST       | Public          | Register a new user (Worker or Employer).                                       |
| `/api/auth/login`                | POST       | Public          | Log in a user and return a JWT token.                                           |

---

### **2. Job Endpoints**
| **Endpoint**                     | **Method** | **Access**      | **Description**                                                                 |
|----------------------------------|------------|-----------------|---------------------------------------------------------------------------------|
| `/api/jobs`                      | POST       | Employer        | Create a new job.                                                               |
| `/api/jobs`                      | GET        | Admin, Employer, Worker | Fetch all jobs.                                                         |
| `/api/jobs/{id}`                 | GET        | Admin, Employer, Worker | Fetch a job by ID.                                                     |
| `/api/jobs/{id}`                 | PUT        | Employer        | Update a job (only the job creator can update it).                              |
| `/api/jobs/{id}`                 | DELETE     | Employer        | Delete a job (only the job creator can delete it).                              |
| `/api/jobs/employer`             | GET        | Employer        | Fetch all jobs created by the logged-in employer.                               |

---

### **3. Job Application Endpoints**
| **Endpoint**                     | **Method** | **Access**      | **Description**                                                                 |
|----------------------------------|------------|-----------------|---------------------------------------------------------------------------------|
| `/api/job-applications/apply/{jobId}` | POST    | Worker        | Apply for a job.                                                                |
| `/api/job-applications/job/{jobId}`  | GET       | Employer      | Fetch all applications for a specific job (only the job creator can access it). |
| `/api/job-applications/{applicationId}/status` | PUT | Employer | Update the status of an application (e.g., approve or reject).          |
| `/api/job-applications/worker`       | GET       | Worker        | Fetch all applications submitted by the logged-in worker.                       |

---

### **4. Admin Endpoints**
| **Endpoint**                     | **Method** | **Access**      | **Description**                                                                 |
|----------------------------------|------------|-----------------|---------------------------------------------------------------------------------|
| `/api/admin/users`               | GET        | Admin           | Fetch all users.                                                                |
| `/api/admin/users/{id}`          | GET        | Admin           | Fetch a user by ID.                                                             |
| `/api/admin/users/{id}`          | DELETE     | Admin           | Delete a user.                                                                  |
| `/api/admin/users/{id}/{newRole}`| PUT        | Admin           | Update a user's role.                                                           |

---

### **5. Profile Endpoints**
| **Endpoint**                     | **Method** | **Access**      | **Description**                                                                 |
|----------------------------------|------------|-----------------|---------------------------------------------------------------------------------|
| `/api/profile`                   | GET        | Admin, Employer, Worker | Fetch the logged-in user's profile.                                     |
| `/api/profile`                   | PUT        | Admin, Employer, Worker | Update the logged-in user's profile.                                     |

---

## **Testing Plan**

### **Step 1: Register Users**
1. **Register an Admin**:
   - **Endpoint**: `POST /api/auth/register`
   - **Body**:
     ```json
     {
       "username": "admin1",
       "password": "password123",
       "email": "admin1@example.com",
       "role": "ADMIN"
     }
     ```

2. **Register an Employer**:
   - **Endpoint**: `POST /api/auth/register`
   - **Body**:
     ```json
     {
       "username": "employer1",
       "password": "password123",
       "email": "employer1@example.com",
       "role": "EMPLOYER"
     }
     ```

3. **Register a Worker**:
   - **Endpoint**: `POST /api/auth/register`
   - **Body**:
     ```json
     {
       "username": "worker1",
       "password": "password123",
       "email": "worker1@example.com",
       "role": "WORKER"
     }
     ```

---

### **Step 2: Login Users**
1. **Login as Admin**:
   - **Endpoint**: `POST /api/auth/login`
   - **Body**:
     ```json
     {
       "username": "admin1",
       "password": "password123"
     }
     ```
   - Save the JWT token for admin.

2. **Login as Employer**:
   - **Endpoint**: `POST /api/auth/login`
   - **Body**:
     ```json
     {
       "username": "employer1",
       "password": "password123"
     }
     ```
   - Save the JWT token for employer.

3. **Login as Worker**:
   - **Endpoint**: `POST /api/auth/login`
   - **Body**:
     ```json
     {
       "username": "worker1",
       "password": "password123"
     }
     ```
   - Save the JWT token for worker.

---

### **Step 3: Test Admin Endpoints**
1. **Fetch All Users**:
   - **Endpoint**: `GET /api/admin/users`
   - **Headers**: `Authorization: Bearer <admin_token>`

2. **Fetch a User by ID**:
   - **Endpoint**: `GET /api/admin/users/{id}`
   - **Headers**: `Authorization: Bearer <admin_token>`

3. **Delete a User**:
   - **Endpoint**: `DELETE /api/admin/users/{id}`
   - **Headers**: `Authorization: Bearer <admin_token>`

4. **Update a User's Role**:
   - **Endpoint**: `PUT /api/admin/users/{id}/{newRole}`
   - **Headers**: `Authorization: Bearer <admin_token>`

---

### **Step 4: Test Employer Endpoints**
1. **Create a Job**:
   - **Endpoint**: `POST /api/jobs`
   - **Headers**: `Authorization: Bearer <employer_token>`
   - **Body**:
     ```json
     {
       "title": "Data Entry Job",
       "description": "Enter data into our system.",
       "category": "DATA_ENTRY",
       "reward": 50.0,
       "status": "OPEN"
     }
     ```

2. **Fetch All Jobs**:
   - **Endpoint**: `GET /api/jobs`
   - **Headers**: `Authorization: Bearer <employer_token>`

3. **Fetch Jobs Created by Employer**:
   - **Endpoint**: `GET /api/jobs/employer`
   - **Headers**: `Authorization: Bearer <employer_token>`

4. **Update a Job**:
   - **Endpoint**: `PUT /api/jobs/{id}`
   - **Headers**: `Authorization: Bearer <employer_token>`
   - **Body**:
     ```json
     {
       "title": "Updated Data Entry Job",
       "description": "Updated description.",
       "category": "DATA_ENTRY",
       "reward": 60.0,
       "status": "IN_PROGRESS"
     }
     ```

5. **Delete a Job**:
   - **Endpoint**: `DELETE /api/jobs/{id}`
   - **Headers**: `Authorization: Bearer <employer_token>`

6. **Fetch Applications for a Job**:
   - **Endpoint**: `GET /api/job-applications/job/{jobId}`
   - **Headers**: `Authorization: Bearer <employer_token>`

7. **Update Application Status**:
   - **Endpoint**: `PUT /api/job-applications/{applicationId}/status?status=APPROVED`
   - **Headers**: `Authorization: Bearer <employer_token>`

---

### **Step 5: Test Worker Endpoints**
1. **Apply for a Job**:
   - **Endpoint**: `POST /api/job-applications/apply/{jobId}`
   - **Headers**: `Authorization: Bearer <worker_token>`

2. **Fetch All Jobs**:
   - **Endpoint**: `GET /api/jobs`
   - **Headers**: `Authorization: Bearer <worker_token>`

3. **Fetch Applications by Worker**:
   - **Endpoint**: `GET /api/job-applications/worker`
   - **Headers**: `Authorization: Bearer <worker_token>`

---

### **Step 6: Test Profile Endpoints**
1. **Fetch Profile**:
   - **Endpoint**: `GET /api/profile`
   - **Headers**: `Authorization: Bearer <token>` (admin, employer, or worker).

2. **Update Profile**:
   - **Endpoint**: `PUT /api/profile`
   - **Headers**: `Authorization: Bearer <token>` (admin, employer, or worker).
   - **Body**:
     ```json
     {
       "email": "updated@example.com",
       "password": "newpassword123"
     }
     ```

---

## **Summary**
This testing plan covers all endpoints and ensures that each role (Admin, Employer, Worker) can access the appropriate endpoints. Let me know if you need further assistance! 🚀
