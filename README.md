# Galaxium Travels Embedded Webchat Example

This repository contains resources for the "galaxium_travels_embedded_webchat_example" project with watsonx Orchestrate Development Edition.

* Galaxium Travels Applications:
    * Galaxium Travels WebApp: http://localhost:8083
    * Galaxium Travels Booking: http://localhost:8082/docs
    * Galaxium Travels HR: http://localhost:8081/docs

* watsonx Orchestrate 
    * Langfuse: http://localhost:3010
    * watsonx Orchestrate LiteChat: http://localhost:3000


* Related resources:

    **IBM Documentation**
    * [IBM watsonx Orchestrate Agent Development Kit](https://developer.watson-orchestrate.ibm.com/)

    **Blog posts**
    * [Integrating watsonx Orchestrate Agent Chat in Web Apps](https://suedbroecker.net/2025/08/08/integrating-watsonx-orchestrate-agent-chat-in-web-apps/)
    * [Getting Started with Local AI Agents in the watsonx Orchestrate Development Edition](https://suedbroecker.net/2025/06/25/getting-started-with-local-ai-agents-in-the-watsonx-orchestrate-developer-edition/)
    

    **The Galaxium Travels Example**   
    * [Galaxium Travels Infrastructure](https://github.com/thomassuedbroecker/galaxium-travels-infrastructure)
    * [Galaxium Travels Idea](https://github.com/Max-Jesch/galaxium-travels)

The steps in the related [YouTube video: TBD]().

## 1. Start your container engine

Ensure your container engine is running on your desktop.

## 2. Set up the example `Galaxium Travels Infrastructure`

### Step 1: Open the first terminal session.

### Step 2: Clone the GitHub repository
```sh
cd example-application-infrastructure
git clone https://github.com/thomassuedbroecker/galaxium-travels-infrastructure
```

### Step 3: Generate environment variables file
```sh
cd galaxium-travels-infrastructure/booking_system_rest
cat .env-template > .env
```

### Step 4: Start all applications with Docker Compose

This command will build and start all applications of the `Galaxium Travels Infrastructure`

```sh
cd ../local-container
bash start-build-containers.sh
```

### Step 6: Stop all applications with Docker compose

Stop all applications; we only need to build and create the container images to verify that everything is working locally. 

>Press 'command' and 'C' in the terminal session.

## 3. Set up the watsons Orchestrate ADK and watsonx Orchestrate Development Server

You can follow the steps in the blog post [Getting Started with Local AI Agents in the watsonx Orchestrate Developer Edition](https://suedbroecker.net/2025/06/25/getting-started-with-local-ai-agents-in-the-watsonx-orchestrate-developer-edition/).

The following steps contain an extraction of the information in the blog post and the [watsonx Orchestrate ADK documentation](https://developer.watson-orchestrate.ibm.com/).

### Step 1: Set up the virtual Python environment

Start a new terminal for in folder **watsonx-orchestrate-adk** and insert the following commands.

```sh
cd /watsonx-orchestrate-adk
python3.12 -m venv .venv
source ./.venv/bin/activate
python3 -m pip install --upgrade pip
```

### Step 2: Install the watsonx Orchestrate CLI

```sh
source ./.venv/bin/activate
python3 -m pip install ibm-watsonx-orchestrate
orchestrate --version
```

### Step 2: Generate the environment file for the ADK

```sh
cat .env_template > .env
```

Configuration for local usage and watsonx.ai.

```sh
# Developer
export WO_DEVELOPER_EDITION_SOURCE=myibm
# IBM ENTITLEMENT
export WO_ENTITLEMENT_KEY=<YOUR_ENTITLEMENT_KEY>
# IBM API KEY
export WATSONX_APIKEY=<YOUR_WATSONX_API_KEY>
export WATSONX_SPACE_ID=<YOUR_SPACE_ID>
```

### Step 3: Start the watsonx Orchestrate Developer Edition

```sh
orchestrate server start --env-file .env
```

### Step 4: Generate an export of the Docker Compose configurations

```sh
orchestrate server eject -e $(pwd)/.env
```

This command generates a `docker-compose.yml` and a `server.env` file in the current folder.

### Step 5: Stop the server

```sh
orchestrate server stop
```

### Step 6: Add the "Galaxium Travels Infrastructure" to the Docker Compose file

Add the following code to the services in the `docker-compose.yml`.

```sh
  wxo-doc-processing-cache-minio-init:
    image: ${OPENSOURCE_REGISTRY_PROXY:-docker.io}/minio/mc:latest
    platform: linux/amd64
    profiles:
      - docproc
    depends_on:
      wxo-server-minio:
        condition: service_healthy
    entrypoint: >
      /bin/sh -c "
      /usr/bin/mc alias set wxo-server-minio http://wxo-server-minio:9000 ${MINIO_ROOT_USER:-minioadmin} ${MINIO_ROOT_PASSWORD:-watsonxorchestrate};
      /usr/bin/mc mb wxo-server-minio/wxo-document-processing-cache;
      exit 0;
      "
  
  ########################
  # Galaxium Travel Infrastructure 
  # ------- begin -------
  ########################

  hr_database:
    image: hr_database:1.0.0
    container_name: wx_hr_database
    ports:
    - 8081:8081

  booking_system:
    image: booking_system_rest:1.0.0
    container_name: wx_booking_system_rest
    ports:
    - 8082:8082

  web_app:
    image: web_app:1.0.0
    container_name: wx_web_app
    ports:
    - 8083:8083
    environment:
    - BACKEND_URL=http://booking_system:8082
    
  ########################
  # Galaxium Travel Infrastructure 
  # ------- begin -------
  ########################

volumes:
  tools:
    driver: local
  wxo-server-redis-data:
    driver: local
  wxo-server-db:
```

### Step 7: Start the watsonx Orchestrate Development Edition Server again

We start the server with [Langfuse](https://github.com/langfuse/langfuse) for monitoring the local [LangGraph](https://github.com/langchain-ai/langgraph) agents.

Therefore we use the generated and configured `server.env` and `docker-compose.yml` file.

```sh
source ./.venv/bin/activate
export SERVER_ENVIRONMENT=server.env
export DOCKER_COMPOSE_FILE=docker-compose.yml
orchestrate server start -e ${SERVER_ENVIRONMENT} -f ${DOCKER_COMPOSE_FILE}  --with-langfuse
```

## 4. Generate a new Agent

### Step 1: Activate local environment

```sh
source ./.venv/bin/activate
orchestrate env activate local
```

## 4. Generate a new Agent

Open a new Terminal in the **watsonx-orchestrate-adk** folder.

### Step 1: Activate local environment

```sh
source ./.venv/bin/activate
orchestrate env activate local
```

### Step 2: Start the "lite chat"

```sh
orchestrate chat start
orchestrate server logs
```

### Step 3: Press create new Agent in the watsonx Orchestrate lite chat 

### Step 4: Define a Name

```text
Manage_Galaxium_User_And_Bookings_Agent
```

### Step 5: Description

Insert the following text into the description.

```text
This Agent manages all the required tasks a user would perform to create a user, book a flight, cancel a booking, and list available flights.
```

### Step 6: Profile

Insert the following text into the profile.

```
This Agent manages all tasks a user would perform to create a user, book a flight, cancel a booking, and list available flights in the Galaxium Travel web application.
```

### Step 6: Behavior 

Insert the following text in the behavior section.

```text
Select the right available tool to fulfill the following tasks for a user who interacts with the Agent:
* create a user
* create a booking
* cancel a booking
* list available flights.
```

### Step 7: Import the given REST API as tools

The `REST API `is documented in an Open API format and can be imported as tools into watsonx Orchestrate Agent.
The prepared `OpenAPI` file: `example-application-infrastructure/galaxium-travels-infrastructure/booking_system_rest/sample-galaxium-booking-backend-openapi-3.0.3-compose.json`

## 5. Verify the Agent works

* Default Agent Architecture

Ask the following questions and see how the Agent responds to your question.

Question: `I want to book a flight?`

## 6. Verify the agent behavior in Langfuse

Now we can inspect the detailed behavior of the Agent in langfuse locally.

## 7. Add the Agent to the "Galaxium Travels" webapp

Now we will change the code in the web application.
Then we will add the changes for the container in the Docker Compose file.

### Step 1: List the agents

```sh
orchestrate agents list
```

### Step 2: Create the `embedded webchat`

```sh
export ENVIRONMENT=draft
export AGENT_NAME="Manage_Galaxium_User_And_Bookings_Agent_8713u4"
source ./.venv/bin/activate
orchestrate channels webchat embed --agent-name=${AGENT_NAME} --env=${ENVIRONMENT}
```

### Step 3: Stop the `watsonx Orchestrate Development Edition server`

```sh
orchestrate server stop
```

### Step 4: Insert the generated code into the `example-application-infrastructure/galaxium-travels-infrastructure/galaxium-booking-web-app/app/templates/index.html` file.

```html
  </style>
  <!-- INSET THE COPIED CODE -->
  <!-- watsonx Orchestrate Agent -->
     YOUR CODE
  <!-- INSET THE COPIED CODE -->
</head>
```

### Step 4: Start the `Galaxium Travels Infrastructure` to rebuild the applications

Open the terminal with the `galaxium-travels-infrastructure`.

```sh
cd example-application-infrastructure/galaxium-travels-infrastructure/local-container
bash build-containers.sh
```

### Step 5: Start the watsonx Orchestrate Developer Edition server again

Select an existing terminal in folder **watsonx-orchestrate-adk**.

```sh
source ./.venv/bin/activate
orchestrate server start -e server.env -f docker-compose.yml  --with-langfuse
```

### Step 6: Start the web chat lite

Select the other existing terminal in folder **watsonx-orchestrate-adk**.

```sh
source ./.venv/bin/activate
orchestrate chat start
orchestrate server logs
```

### Step 7: Verify the Agent is available in the web ui

Open the `Galaxium Travels Web` UI: http://localhost:8083