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

## 2. [Set up the example `Galaxium Travels Infrastructure`](/galaxium_setup.md)

## 3. [Set up the watsons Orchestrate ADK and watsonx Orchestrate Development Server](/2-watsonx_adk_setup.md)

## 4. [Generate a new Agent](/3-create_an_agent.md)

## 5. Verify the Agent works

* Default Agent Architecture

Ask the following questions and see how the Agent responds to your question.

Question: `I want to book a flight?`

## 6. Verify the agent behavior in Langfuse

Now we can inspect the detailed behavior of the Agent in [langfuse locally](http://localhost:3010). (orchestrate@ibm.com/orchestrate)

## 7. [Add the Agent to the "Galaxium Travels" webapp](/4-embed_webchat.md)

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