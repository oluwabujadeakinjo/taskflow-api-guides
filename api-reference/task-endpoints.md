# TaskFlow API reference
This API reference details the endpoints available within the TaskFlow platform. Use this documentation to integrate task management, project tracking, and workflow automation features into frontend applications or backend microservices.

## Global configurations
### Base URL
All requests to the TaskFlow API must target the following baseline routing layer:
`[https://api.example.com/v1](https://api.example.com/v1)`

### Authentication
Every endpoint within this reference—excluding public routing contracts—requires a valid JSON Web Token (JWT) provided inside the request header.

**Header format:**
`Authorization: Bearer <acces_token>`

## Tasks endpoint

### List all tasks
Retrieves a filtered collection of tasks assigned to or created by the authenticated user account.

- **HTTP method:** `GET`
- **Path:** `/tasks`

**Query parameters**

|**Parameter**|**Type**|**Required/Optional**|**Descrption**|
|-------------|--------|---------------------|--------------|
|`status`|string|Optional|Filters results by execution states (`pending`, `in_progress`, `completed`).|
|`limit`|integer|Optional|Specifies the maximum number of task records to return. Default is `20`.|

**Request example**

```Bash
curl -X GET https://api.example.com/v1/tasks?status=in_progress \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" \
-H "Accept: application/json"
```

**Success response (200 OK)**

```JSON
{
    "task_id": "t_98321",
    "title": "Implement login API documentation",
    "description": "Write standard-compliant reference docs for the internal login endpoint.",
    "status": "in_progress",
    "assigned_to": "u_5348",
    "created_at": "2026-05-18T10:00:00Z"
}
```

### Create a task
Generates a new task entity inside the designated project workspace and assigns operational ownership.

- **HTTP method:** `POST`
- **Path:** `/tasks`

**Request body parameters**

|**Parameter**|**Type**|**Required/Optional**|**Description**|
|-------------|--------|---------------------|---------------|
|`title`|string|**Required**|The concise title detailing the purpose of the task.|
|`description`|string|Optional|Extended text outlining criteria, context, or requirements.|
|`project_id`|string|**Required**|The unique system identifier for the associated project workspace.|

**Request example**
```Bash
curl -X POST https://api.example.com/v1/tasks \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" \
-H "Content-Type: application/json" \
-d '{
    "title": "Configure log rotation",
    "description": "Deploy pm2-logrotate on production servers to optimize storage volumes.",
    "project_id": "p_4412"
}'
```

**Success response (201 Created)**
```JSON
{
    "task_id": "t_98322",
    "title": "Configure log rotation",
    "description": "Deploy pm2-logrotate on production servers to optimize storage volumes.",
    "status": "pending",
    "project_id": "p_4412",
    "created_at": "2026-05-18T16:45:00Z"
}
```

### Update a task status
Modifies the life cycle execution state of an existing task.

- **HTTP method:** `PATCH`
- **Path:** `/tasks/{task_id}/status`

**Path parameters**
|**Parameter**|**Type**|**Description**|
|-------------|--------|---------------|
|`task_id`|string|The unique identifier string of the specific target task.|

**Request body parameters**
|**Parameter**|**Type**|**Required/Optional**|**Description**|
|-------------|--------|---------------------|---------------|
|`status`|string|**Required**|The updated state assignment. Accepted inputs: `pending`, `in_progress`, `completed`.|

**Request example**
```Bash
curl -X PATCH https://api.example.com/v1/tasks/t_98322/status \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" \
-H "Content-Type: application/json" \
-d '{
    "status": "completed"
}'
```

**Success response (200 OK)**
```JSON
{
    "task_id": "t_98322",
    "status": "completed",
    "updated_at": "2026-05-18T16:50:00Z"
}
```

## Global error responses
When an API call fails, the server responds with a standardized JSON schema.

**Unauthorized token error (401 Unauthorized)**
```JSON
{
    "status": 401,
    "error": "invalid_credentials",
    "message": "The access token provided is invalid or has expired. Request a new token."
}
```

**Resource validation failure (422 Unprocessable Entity)**
```JSON
{
    "status": 422,
    "error": "validation_failed",
    "message": "The status parameter must match one of the allowed types."
}
```

Review the full error portfolio inside the [**Error handling guide**]().