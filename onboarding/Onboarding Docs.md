# TaskFlow API onboarding guide
---
## Overview
Welcome to the TaskFlow API oboarding guide.

---

This document introduces developers to the TaskFlow API ecosystem and explains how to:
- understand the platform architectue;
- install and configure the API locally;
- authtenticate requests;
- test endpoints;
- inspect logs;
- troubleshoot common issues; and
- begin contributing to the development workflow.

By the end of this guide, you should be able to run TaskFlow API locally, interact with protected endpoints, and understand the operational workflow used in development and production environments.

---

## TaskFlow API
TaskFlow API is a REST API designed for:
- task management;
- user authentication;
- project collaboration;
- activity tracking; and
- workflow automation.
---
It exposes endpoints to allow client application:
- create and manage tasks;
- authenticate users;
- organize projects;
- assign responsibilities;
- track progress; and
- monitor application health.
---

It follows a client-server procedure where:
1. Client applications send HTTP requests;
2. The API processes business logic;
3. PostgreSQL stores persistent data; and
4. JSON responses are returned to clients.
---

## System architecture overview
The TaskFlow platform contains the following core components:

|**Component**|**Responsibility**|
|-------------|------------------|
|API server|Handles requests, authentication, and business logic|
|PostgreSQL database|Stores users, tasks, and project data|
|PM2|Manages Node.js application processes|
|Docker|Provides containerized deployment environments|
|Monitoring services|Tracks uptime, logs, and application health|
---

## Development workflow
The standard development workflow follows this sequence:
1. Clone the repository;
2. Install project dependencies;
3. Configure environment variables;
4. Start PostgreSQL;
5. Run the API locally;
6. Test endpoints using cURL; and
7. Monitor logs and application health.
---
## Prerequisites
Before starting, ensure the following tools are installed:
- Node.js 18+
- PostgreSQL 14+
- Docker
- PM2
- Git
- cURL
---
You also need:
- terminal access;
- administrator permissions;
- internet connectivity; and
- a text editor.
---
## Install and configure TaskFlow API locally
---
### Step 1: Clone the repository
Clone the TaskFlow API repository from GitHub:

```Bash
git clone https://github.com/example/taskflow-api.git
```
---
Move into the project directory:

```Bash
cd taskflow-api
```
---
### Step 2: Install dependencies
Install all required project dependencies:

```Bash
npm install
```
> This command installs packages defined in the `package.json` file.
---
### Step 3: Configure environment variables
**Copy the environment file**

```Bash
cp .env.example .env
```
---
**Configure the variables**
Open the `.env` file and update the following variables:

|**Variables**|**Description**|
|-------------|---------------|
|`PORT`|Port used by the API server|
|`DB_URL`|PostgreSQL connection string|
|`JWT_SECRET`|Secret used to sign authentication tokens|
|`NODE_ENV`|Application environment (`development` or `production`)|

---
Example:
```Bash
PORT = 3000
DB_URL = postgresql://user:password@localhost:5432/taskflow
JWT_SECRET = super_secret_key
NODE_ENV = development
```
> **Security note:** Never commit `.env` files to version control systems.
---
### Step 4: Start PostgreSQL
Ensure the PostgreSQL service is running before starting the API:

```Bash
sudo systemctl start postgresql
```
---
Verify the service status:

```Bash
sudo systemctl status postgresql
```
---
### Step 5: Start the API server
Launch the application in development mode:

```Bash
npm run dev
```
---
Expected output:

```Bash
Server running on http://localhost:3000
Database connection established successfully
```
---
### Verify API health
Send a request to the health endpoint to verify the service is operational:

```Bash
curl http://localhost:3000/health
```
---
Expected response:

```JSON
{
    "status": "healthy",
    "timestamp": "2026-05-13T22:50:07Z",
    "uptime_seconds": "172800"
}
```
---
### Step 7: Authentiicate requests
TaskFlow API uses token-based authentication.

---
**Endpoint:**
`POST /auth/login`

---
**Request:**

```Bash
curl -X POST http://localhost:3000/auth/login \
-H "Content-Type: application/json" \
-d '{
    "username": "isaac_tiothy",
    "password": "strong_password"
}'
```
---
**Expected response:**

```JSON
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9",
    "expires_in": 3600
}
```
---
### Step 8: Access protected endpoints
Use the generated token in subsequent requests:

```Bash
curl -X GET http://localhost:3000/tasks \
-H "Authorization: Bearer <access_token>"
```
---
### Step 9: Monitor logs and application health
**PM2 Monitoring**
View managed processes:

```Bash
pm2 status
```
---
Enable the real-time monitoring dashboard:

```Bash
pm2 monit
```
---
View logs:

```Bash
pm2 logs taskflow-api
```
---
**Docker logs**
List running containers:

```Bash
docker ps
```
---
View container logs:

```Bash
docker logs <container-id>
```
---
## Error handling
See the table below for common troubleshooting scenarios:

|**Issue**|**Possible Cause**|**Solution**|
|---------|------------------|------------|
|API fails to start|Missing environment variables|Verify `.env` configuration|
|Database connection refused|PostgreSQL service offline|Restart PostgreSQL|
|Authentication fails|Invalid credentials|Verify username and password|
|Port already in use|Another service occupies port `3000`|Change the `PORT` variable|
|Container exits immediately|Invalid startup command|Inspect Docker logs|

---
## Recommended security practices
Follow these recommendations when working with TaaskFlow API:
- Use HTTPS in production environments.
- Avoid hardcoding credentials.
- Restrict database access.
- Rotate authentication secrets periodically.
- Restrict access to logs and environment files.
- Monitor failed authentication attempts.
---
## Contribution workflow
Developers contributing to TaskFlow API should follow this workflow:
1. Pull the latest changes;
2. Create a feature branch;
3. Implement changes;
4. Test functionality locally; and
5. Submit a pull request.
---
Pull request example:

```Bash
git checkout -b feature/task-filtering
```
---
## Additional resources

|**Resource**|**Purpose**|
|------------|-----------|
|[API reference documentation]()|Endpoint specifications|
|[Development guide]()|Production deployment workflow|
|[Monitoring guide]()|Logging and observability workflows|
|[Error handling documentation]()|Common API errors and responses|

