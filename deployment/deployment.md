# Deploying TaskFlow API to production

Configure and deploy the TaskFlow API using Docker and PM2. This process ensures functionality by thorough error diagnosis and prevention.

## Prerequisites
* Linux server
* Node.js 18+
* Docker
* Git
* Project Manager 2 (PM2)
* PostgreSQL 14+
* Environment variables configured

## Step 1: Clone the repository
Invoke Git and clone the repository:

```Bash
# Use the clone command to clone the repo from GitHub
git clone https://github.com/example/taskflow-api.git
```

## Step 2: Configure environment repository
TaskFlow API uses environment variables to manage sensitive configurations. Store them in a hidden file named `.env`.

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

## Step 3: Deploy using Docker
Create and launch the application in a container, using the `docker build` and `docker run` commands.

### Create a docker image
Use `docker build` to create a template (Image):

```Bash
docker build -t taskflow-api .
```
### Create and start a container
Use `docker run` to create and start a new container from an image:

```Bash
docker run -d -p 3000:3000 taskflow-api
```

### Command breakdown

|**Command/Flag**|**Description**|
|----------------|---------------|
|`docker build`|Tells `Docker` to create an image|
|`docker run`|Tells `Docker` to create and start a container from an image|
|`-t`|Gives the image a name (`taskflow-api`) for future referencing|
|`.`|Tells `Docker` that the Dockerfile and all the files it needs to copy are located in the current directory|
|`-d`|Runs the container in the background|
|`-p 3000:3000`|Maps your computer's port to the container's port. The first `3000` is the port on your machine|

## Step 4: Deploy using PM2
Ensure the application stays online indefinitely and utilizes all available system resources.

### Launch the application
Use `PM2` to launch the application and give it a specific name:

```Bash
# Run server.js and name the process taskflow-api
pm2 start server.js --name taskflow-api
```

### Check the application status
Use `pm2 status` to know if your application is online:

```Bash
pm2 status
```

## Step 5: Verify deployment
Send a request to the health endpoint to verify that the service is operational:

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
|--------------|--------------|
|**Port already in use**|Port `3000` is occupied.|Change the `PORT` variable in your `.env` file to an available port.|
|**Database connection failed**|The API cannot reach the PostgreSQL server.|Ensure PostgreSQL is running and your `DB_URL` in the `.env` file matches your local credentials.|
|**Missing environment variables**|The server crashes because a required key is undefined.|Verify that your `.env` file contains all the variables listed in the `.env.example` template.|
|**Container exits immediately**|The Docker container stops right after `docker run`.|Check logs using`docker logs [container_id].|
|**Database unavailable (Docker)**|The API container starts before the database container is ready.|Use a "wait-for-it" script or the `depends_on` condition with `healthcheck` in your `docker-compose.yml` to ensure the database is accepting connections first.|

> **Pro-Tip:** If you encounter the "**Container exits immediately**" error, keep the container alive by overriding the entrypoint:
``` Bash
 docker run -it --entrypoint /bin/sh taskflow-api
```
This allows you to manually run `npm run dev` inside the container to see the exact error message that is causing the shutdown.

## Security recommendations
* Ensure all requests are sent in HTTPS (Secure HTTP).
* Ensure you configure firewall.
* Ensure secure secrets management.
* Avoid hardcoding credentials.

## Next steps
Now that the TaskFlow API is deployed:

* Check the [**API docs**](apireference.md) to know more about the API.
* Check the [**monitoring docs**](authentication.md) to learn how to track the availability, performance, functionality, and security of the API.
* For further reading, go through the [**scaling/deployment resources**](deployment.md).