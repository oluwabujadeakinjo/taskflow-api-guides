# TaskFlow API onboarding guide
Welcome to the TaskFlow API onboarding guide.
This document introduces new developers to the TaskFlow API ecosystem. Use this guide to:

- undertand system architecture;
- configure local development environments;
- authenticate requests; and
- integrate changes into the core database.

By the end of this guide, you will be able to: 
- run the TaskFlow API locally;
- interact with protected endpoints; and
- navigate the standard development workflow.

## Platform overview
TaskFlow API is a REST API designed for the following core workflows:
- Task management and life cycle tracking
- User token-based authentication
- Project collaboration and ownership assignment
- System activity tracking and performance metrics

It operates on a client-server model where:
1. Client applications send HTTP requests to exposed endpoints
2. The API server processes requests against core workflows
3. PostgreSQL stores persistent data
4. The server returns JSON responses are returned to client applications

## System architecture overview
The TaskFlow platform relies on these structural components:

|**Component**|**Responsibility**|
|-------------|------------------|
|API server|Handles requests, authentication, and business logic|
|PostgreSQL database|Stores users, tasks, and project data|
|PM2|Manages Node.js application processes|
|Docker|Provides containerized deployment environments|
|Monitoring services|Tracks uptime, logs, and application health|

## Development workflow
The standard development workflow follows this step-by-step procedures:
1. Clone the master repository branch locally.
2. Install underlying project dependencies using package managers.
3. Establish localized environment variables.
4. Initialize the background database instance.
5. Launch the local API runtime service.
6. Verify endpoint responses using command-line interface tools.
7. Track live application behavior through runtime logging outputs.

## Prerequisites
Ensure your local workstation has the following dependencies installed before beginning setup:
- Node.js 18+
- PostgreSQL 14+
- Docker
- PM2
- Git
- cURL

Operational workflow requirements:
- Terminal emulator access with administrative command permissions
- Active network access for pulling remote dependency structures
- A code editor or integrated development environment (IDE)

## Install and configure TaskFlow API locally

### Step 1: Clone the repository
Clone the TaskFlow API repository from your version control workspace:

```Bash
git clone https://github.com/example/taskflow-api.git
```

Move into the project directory:

```Bash
cd taskflow-api
```

### Step 2: Install dependencies
Install all required project dependencies:

```Bash
npm install
```
> This command installs packages defined in the `package.json` file.

### Step 3: Configure environment variables
Copy the environment file:

```Bash
cp .env.example .env
```
Open the newly created `.env` file and update the following variables:

|**Variables**|**Description**|
|-------------|---------------|
|`PORT`|Port used by the API server|
|`DB_URL`|PostgreSQL connection string|
|`JWT_SECRET`|Secret used to sign authentication tokens|
|`NODE_ENV`|Application environment (`development` or `production`)|

Configuration structure example:
```Bash
PORT = 3000
DB_URL = postgresql://user:password@localhost:5432/taskflow
JWT_SECRET = super_secret_key
NODE_ENV = development
```
> **Security note:** Never commit `.env` files to version control workspaces.

### Step 4: Initialize the database service
Ensure the PostgreSQL service is running before starting the API:

```Bash
sudo systemctl start postgresql
```

Verify the service status:

```Bash
sudo systemctl status postgresql
```

### Step 5: Start the API server
Launch the application in development mode:

```Bash
npm run dev
```

Expected output:

```Bash
Server running on http://localhost:3000
Database connection established successfully
```

### Step 6: Verify API health
Send a request to the health endpoint to verify the service is operational:

```Bash
curl http://localhost:3000/health
```

Expected response:

```JSON
{
    "status": "healthy",
    "timestamp": "2026-05-13T22:50:07Z",
    "uptime_seconds": "172800"
}
```

### Step 7: Authentiicate requests
TaskFlow API secures endpoints using JSON Web Token (JWT) parameter validations. Dispatched user credentials generate short-lived tokens.

- **Endpoint:** `POST /auth/login`

Send an authentication login request, using command-line quesry strings:

```Bash
curl -X POST http://localhost:3000/auth/login \
-H "Content-Type: application/json" \
-d '{
    "username": "isaac_tiothy",
    "password": "strong_password"
}'
```

**Expected response:**

```JSON
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9",
    "expires_in": 3600
}
```

### Step 8: Access protected endpoints
Use the generated token in subsequent requests:

```Bash
curl -X GET http://localhost:3000/tasks \
-H "Authorization: Bearer <access_token>"
```
> The generated token is passed directly into the request headers of subsequesnt requests.

### Step 9: Monitor logs and application health
**Process monitoring with PM2**
View managed processes:

```Bash
pm2 status
```
Enable the real-time monitoring dashboard:

```Bash
pm2 monit
```
Stream logs continually:

```Bash
pm2 logs taskflow-api
```

**Container monitoring with Docker**
List running containers:

```Bash
docker ps
```

View container logs:

```Bash
docker logs <container-id>
```

## Troubleshooting operational errors
See the table below for standard local compilation faults:

|**Issue**|**Root Cause**|**Solution**|
|---------|------------------|------------|
|**API fails to start**|Missing environment variables|Verify `.env` configuration|
|**Database connection refused**|PostgreSQL service offline|Restart PostgreSQL|
|**Authentication fails**|Invalid credentials|Verify username and password|
|**Port already in use**|Another service occupies port `3000`|Change the `PORT` variable|
|**Container exits immediately**|Invalid startup command|Inspect Docker logs|

## Recommended security practices
Follow these recommendations when working with TaskFlow API:
- Use HTTPS in production environments
- Avoid hardcoding credentials
- Restrict database access
- Rotate authentication secrets periodically
- Restrict access to logs and environment files
- Monitor failed authentication attempts

## Core contribution procedures
Developers contributing to TaskFlow API should follow this workflow:
1. Pull the latest changes from the origin development main branch.
2. Initialize separate descriptive feature tracking branches (`git checkout -b feature/name`).
3. Run technical logic updates and structural modifications within working directories.
4. Execute localized verification health testing checks to ensure stability.
5. Open up a formal code pull request for architectural code evaluations.

Branch initialization standard formatting example:

```Bash
git checkout -b feature/task-filtering
```

## Additional support documentation

|**Resource**|**Purpose**|
|------------|-----------|
|[API reference documentation]()|Endpoint specifications|
|[Deploypment guide](https://github.com/oluwabujadeakinjo/taskflow-api-guides/blob/main/deployment/deployment.md)|Production deployment workflow|
|[Monitoring guide](https://github.com/oluwabujadeakinjo/taskflow-api-guides/blob/main/operations/monitoring%20and%20logging.md)|Logging and observability workflows|

## Next steps
Now that you have complet onboarding, you should be able to:
- run TaskFlow API locally;
- authenticate requests;
- inspects logs;
- monitor service health; and
- troubleshoot common operational issues.

Recommended next topics:
- [scaling infrastructure]();
- [CI/CD automation]();
- [incident response procedures]().
