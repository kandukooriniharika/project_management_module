Project Management System - API Documentation
Overview
This document details all API endpoints for the Project Management System, grouped by functionality and role.
All endpoints share the base URL:

Base URL: /api/projects

📋 Table of Contents
Project Operations

Task Operations

User & Role Operations

Reporting Operations

Response Format

Error Handling

Business Rules

🗂 Project Operations
1. Create Project
Endpoint: POST /api/projects

Description: Create a new project

Request Body: ProjectDTO

Response: ApiResponse<Project>

Functionality:

Saves a new project with initial metadata

Assigns owner and default status as ACTIVE

Optionally assigns initial team members

Authorization: Admin / Project Manager

Status Codes:

200 OK – Project created

400 Bad Request – Validation failed

500 Internal Server Error – System error

2. Update Project
Endpoint: PUT /api/projects/{projectId}

Description: Update existing project details

Path Parameters: projectId – Project ID

Request Body: ProjectDTO

Response: ApiResponse<Project>

Functionality:

Allows editing name, description, deadlines, status

Prevents updates to archived projects

Authorization: Project Owner / Admin

Status Codes:

200 OK – Updated successfully

400 Bad Request – Validation failed

404 Not Found – Project not found

3. Get Project Details
Endpoint: GET /api/projects/{projectId}

Description: Retrieve detailed information about a project

Response: ApiResponse<Project>

Authorization: Project members / Admin

Status Codes:

200 OK – Found

404 Not Found – Project not found

4. Archive Project
Endpoint: PUT /api/projects/{projectId}/archive

Description: Archive a project (read-only)

Response: ApiResponse<Project>

Authorization: Admin / Project Owner

Status Codes:

200 OK – Archived

400 Bad Request – Already archived

✅ Task Operations
5. Create Task
Endpoint: POST /api/projects/{projectId}/tasks

Description: Add a new task under a project

Request Body: TaskDTO

Response: ApiResponse<Task>

Functionality:

Associates task with specified project

Assigns status as TODO by default

Authorization: Project Members with edit rights

6. Update Task
Endpoint: PUT /api/tasks/{taskId}

Description: Edit an existing task

Response: ApiResponse<Task>

Authorization: Task Assignee / Project Manager

7. Change Task Status
Endpoint: PUT /api/tasks/{taskId}/status

Query Parameters: status – NEW / IN_PROGRESS / DONE

Response: ApiResponse<Task>

Authorization: Task Assignee / Project Manager

8. Get All Tasks in Project
Endpoint: GET /api/projects/{projectId}/tasks

Response: ApiResponse<List<Task>>

Authorization: Project Members

👥 User & Role Operations
9. Assign User to Project
Endpoint: POST /api/projects/{projectId}/members

Description: Add a user to the project team

Request Body: { "userId": "string", "role": "MEMBER|MANAGER" }

Response: ApiResponse<ProjectMember>

Authorization: Admin / Project Manager

10. Remove User from Project
Endpoint: DELETE /api/projects/{projectId}/members/{userId}

Authorization: Admin / Project Manager

📊 Reporting Operations
11. Project Summary
Endpoint: GET /api/projects/{projectId}/summary

Description: Fetch high-level metrics for a project (tasks, deadlines, progress)

Authorization: Project Members

12. All Projects Report
Endpoint: GET /api/projects/report

Description: Returns summary of all active projects

Authorization: Admin

📄 Response Format
All responses use ApiResponse<T>:

json
Copy
Edit
{
  "success": true,
  "message": "string",
  "data": T | null
}
⚠ Error Handling
200 OK – Success

400 Bad Request – Validation errors

401 Unauthorized – No authentication

403 Forbidden – No permission

404 Not Found – Resource missing

500 Internal Server Error – Unexpected error

📝 Business Rules
Archived projects cannot be edited or have new tasks created.

Only Admin/Project Owner can archive a project.

Tasks must have a start date before due date.

Only task assignee or project manager can change task status.
