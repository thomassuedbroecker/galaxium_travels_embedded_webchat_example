# Add the Agent to the "Galaxium Travels" webapp

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

[langfuse locally](http://localhost:3010). (orchestrate@ibm.com/orchestrate)

### Step 6: Start the web chat lite

Select the other existing terminal in folder **watsonx-orchestrate-adk**.

```sh
source ./.venv/bin/activate
orchestrate chat start
orchestrate server logs
```

### Step 7: Verify the Agent is available in the web ui

Open the `Galaxium Travels Web` UI: http://localhost:8083

### [Home](https://github.com/thomassuedbroecker/galaxium_travels_embedded_webchat_example/blob/main/README.md)