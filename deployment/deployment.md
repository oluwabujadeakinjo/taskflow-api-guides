# Deploying TaskFlow API to production

Configure and deploy the TaskFlow API to a production environment. This guide offers two alternative development pathways:
- containerization using Docker
- process management using PM2

## Prerequisites
Before deploying, ensure your server has the following dependencies installed:
* Linux server (Ubuntu 22.04 LTS or similar)
* Node.js 18+
* Docker Engine
* Git
* Project Manager 2 (PM2)
* PostgreSQL 14+
* A configured `.env` file (see step 2)

## Step 1: Clone the repository
Clone the project repository from GitHub to your local server directory:

```Bash
# Use the clone command to clone the repo from GitHub
git clone https://github.com/example/taskflow-api.git
```

## Step 2: Configure environment variables
TaskFlow API uses environment variables to manage sensitive configurations (such as database credentials and application states.) Store these configurations in a secure `.env` file in the project root.

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
|`PORT`|`3000`|The port the server will answer on.|
|`DB_URL`|`postgresql://user:pass@localhost:5432/taskflow`|PostgreSQL connection string.|
|`JWT_SECRET`|`your_super_secret_key`|Secret key for signing tokens.|
|`NODE_ENV`|`development`|Dictates the environment state. Set it to `production`.|

## Step 3: Choose a deployment method
Select one of the following production deployment strategies:

### Option A: Deploy using Docker
Containerization bundles the application code with its specific dependencies to ensure consistent behavior across environments.

**1. Build the docker image**
Run `docker build` to create a template image named `taskflow-api`:

```Bash
docker build -t taskflow-api .
```
**2. Start the container**
Run `docker run` to launch a cotainer instance:

```Bash
docker run -d -p 3000:3000 taskflow-api
```

**Docker command breakdown**

|**Command/Flag**|**Description**|
|----------------|---------------|
|`docker build`|Tells `Docker` to create an image|
|`docker run`|Tells `Docker` to create and start a container from an image|
|`-t`|Gives the image a name (`taskflow-api`) for future referencing|
|`.`|Tells `Docker` that the Dockerfile and all the files it needs to copy are located in the current directory|
|`-d`|Runs the container in the background|
|`-p 3000:3000`|Maps your computer's port to the container's port. The first `3000` is the port on your machine|

### Option B: Deploy using PM2
PM2 is a production process manager that keeps the application online indefinitely and utilizes all available system resources.

**1. Launch the application process**
Run `PM2` to launch the application and give it a distinct name (`taskflow-api`):

```Bash
# Run server.js and name the process taskflow-api
pm2 start server.js --name taskflow-api
```

**2. Verify process status**
Run `pm2 status` to review the live status of application instances:

```Bash
pm2 status
```

## Step 4: Verify the deployment
Send a request to the `/health` endpoint to ensure the service is operational:

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

## Step 6: Troubleshooting (optional)
If the application fails to start, check for these common configuration issues:

|**Issues**|**Description**|**Solution**|
|----------|---------------|------------|
|**Port already in use**|Port `3000` is occupied.|Change the `PORT` variable in your `.env` file to an available port.|
|**Database connection failed**|The API cannot reach the PostgreSQL server.|Ensure PostgreSQL is running and your `DB_URL` in the `.env` file matches your local credentials.|
|**Missing environment variables**|The server crashes because a required key is undefined.|Verify that your `.env` file contains all the variables listed in the `.env.example` template.|
|**Container exits immediately**|The Docker container stops right after `docker run`.|Check logs using`docker logs [container_id].|
|**Database unavailable (Docker)**|The API container starts before the database container is ready.|Use a "wait-for-it" script or the `depends_on` condition with `healthcheck` in your `docker-compose.yml` to ensure the database is accepting connections first.|

> **Pro-Tip:** If you encounter the "**Container exits immediately**" error, keep the container alive by overriding the entrypoint:
``` Bash
 docker run -it --entrypoint /bin/sh taskflow-api
```
> This allows you to manually run `npm run dev` inside the container to see the exact error message that is causing the shutdown.

## Security recommendations
* Enforce HTTPS transport layers globally to encrypt data in transit.
* Configure host-level firewalls to isolate database access from public web requests.
* Never commit the production `.env` file into open-source version control tracking.
* Never hardcode credentials.

## Next steps
Now that the TaskFlow API is deployed:

* Review the [**API documentation**](apireference.md) to explore the system's available end-point layouts.
* Read the [**Monitoring and logging guide**](https://github.com/oluwabujadeakinjo/taskflow-api-guides/blob/main/operations/monitoring%20and%20logging.md) to implement metric tracing for system infrastructure.
