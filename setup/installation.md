# Installing and configuring TaskFlow API using Bash

This guide walks you through cloning, configuring, and executing a localized installation of the TaskFlow REST API within a Unix-like shell emvironment.

## Prerequisites
Ensure your development workspace has the following dependencies:
* Node.js 18+
* PostgreSQL 14+
* Git command-line interface
* A terminal interface and text editor

## Step 1: Clone the repository
Clone the master project codebase from GitHub into your local working directory:

```Bash
git clone https://github.com/example/taskflow-api.git
```
Navigate into the root repository workspace folder:

```Bash
cd taskflow-api
```

## Step 2: Install project dependencies
Initialize the node package manager (`npm`) to pull down backend dependencies defined within the core runtime engine layout:

```Bash
# install the repository
npm install 
```
> **Note:** This command reviews the project's `package.json` file to download and link required sub-modules.

## Step 3: Configure environment variables
TaskFlow API handles sensitive infrastucture parameters through runtime environment adjustments. Save these configurations locally inside an isolated, hidden file named `.env`.

### Create the .env configuration file
Generate your localized environment file by duplicating the provided upstream configuration template:

```Bash
# Replace .env.example with your file name
cp .env.example .env
```

### Edit file configurations
Open the local `.env` properties layout inside a text editor and adjust the target variables to reflect your workstation setup:

|**Variable**|**Default Value**|**Description**|
|------------|-----------------|---------------|
|`PORT`|`3000`|The port the server will listen on|
|`DB_URL`|`postgresql://user:pass@localhost:5432/taskflow`|PostgreSQL connection string|
|`JWT_SECRET`|`your_super_secret_key`|Secret key for signing tokens|
> **Security tip:** Never commit your `.env` file to version control. Ensure it is listed in your `.gitignore` file to prevent leaking credentials to GitHub.

## Step 4: Launch the local server
Launch the application in development mode:

```Bash
npm run dev
```
**Expected Output**
The terminal displays the active port and database connection status:
> - `Server running on http://localhost:3000`
> - `Database connection established successfully`

## Step 5: Verify installation
Validate the API's responsiveness by sending a request the health endpoint:

```Bash
# ping the health check endpoint
curl http://localhost:3000/health
```
**Expected JSON payload response:**
```JSON
{
    "status": "ok",
    "version": "1.2.4",
    "uptime": "120s"
}
```

## Step 6: Troubleshooting installation errors
If the application fails to start, check the table below for common configuration issues:

|**Description**|**Root cause**|**Solution**|
|---------=-----|----------------|------------|
|**Port already in use**|Port `3000` is occupied|Change the `PORT` variable in your `.env` file to an available port.|
|**Database connection failed**|The server cannot connect with the PostgreSQL background process|Ensure PostgreSQL is running and your `DB_URL` in the `.env` file matches your local credentials.|
|**Missing environment variables**|The process crashes on launch|If the server fails to start, verify that your `.env` file contains all the variables listed in the `.env.example` template.|

## Next steps
Now that the local TaskFlow API is running smoothly, continue building out your platform knowledge using the following technical resources:
* Read the [**Developer onboarding guide**](apireference.md) to explore application design frameworks and core architectures.
* Reference the [**Production deployment guide**](authentication.md) to manage system deployments using containerized platforms like **Docker** or **PM2**.
* Review the [**Monitoring and logging guide**](deployment.md) to establlish live metric tracing workflows on application infrastructure.
