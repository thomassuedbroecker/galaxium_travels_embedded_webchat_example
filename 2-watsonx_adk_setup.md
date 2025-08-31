# Set up the watsons Orchestrate ADK and watsonx Orchestrate Development Server


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

### [Home](https://github.com/thomassuedbroecker/galaxium_travels_embedded_webchat_example/blob/main/README.md)