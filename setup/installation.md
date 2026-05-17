# Installing and configuring TaskFlow API using Bash

Install and configure the TaskFlow REST (Representational State Transfer) API using Bash. This process ensure security by implementing a local version control loop.

## Prerequisites
You need:
* Node.js 18+
* PostgreSQL 14+
* Git
* A text editor

## Step 1: Clone the repository
Invoke Git and clone the repository from Github:

```Bash
git clone https://github.com/example/taskflow-api.git
```

## Step 2: Install dependencies
Install using the `npm` package manager:

```Bash
# install the repository
npm install 
```
> This installs all dependencies listed in `package.json`.

## Step 3: Configure environment variables
TaskFlow API uses environment variables to manage sensitive configurations. Instead of hardcoding these values, store them in a hidden file named `.env`.

### Create the .env file
Create a new file in the root directory of the project:

```Bash
# Replace .env.example with your file name
cp .env.example .env
```
### Edit the configuration
Open the `.env` file in your text editor and update the following fields:

|**Variable**|**Default Value**|**Description**|
|------------|-----------------|---------------|
|`PORT`|`3000`|The port the server will listen on|
|`DB_URL`|`postgresql://user:pass@localhost:5432/taskflow`|PostgreSQL connection string|
|`JWT_SECRET`|`your_super_secret_key`|Secret key for signing tokens|
> **Security tip:** Never commit your `.env` file to version control. Ensure it is listed in your `.gitignore` file to prevent leaking credentials to GitHub.

## Step 4: Start the server
Launch the application in development mode:

```Bash
npm run dev
```
**Expected Output**
The terminal displays the active port and database connection status:
> - `Server running on http://localhost:3000`
> - `Database connection established successfully`

## Step 5: Verify installation
Use `curl` to confirm the API is responding correctly by sending a request the health check endpoint:

```Bash
# ping the health check endpoint
curl http://localhost:3000/health
```
**Expected response:**
```JSON
{
    "status": "ok",
    "version": "1.2.4",
    "uptime": "120s"
}
```

## Step 6: Troubleshooting
If the application fails to start, check for these common configuration issues:

|**Description**|**Solution**|
|--------------|--------------|
|**Port already in use**|If port `3000` is occupied, change the `PORT` variable in your `.env` file to an available port.|
|**Database connection failed**|Ensure PostgreSQL is running and your `DB_URL` in the `.env` file matches your local credentials.|
|**Missing environment variables**|If the server fails to start, verify that your `.env` file contains all the variables listed in the `.env.example` template.|

## Next steps
Now that the TaskFlow API is running locally, you can explore the following resources to build your application:
* Check the [**API Endpoint Reference**](apireference.md) to view the full list of all available REST endpoints and parameters.
* Check the [**Authentication Guide**](authentication.md) to learn how to generate and use JWT tokens for secured routes.
* Go through the [**Deployment Documentation**](deployment.md) for instructions for moving your TaskFlow instance from a local environment to a production server using **Docker** or **PM2.**