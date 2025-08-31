
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

### Step 5: Description / Profile

Insert the following text into the description.

```text
This Agent manages all the required tasks a user would perform to create a user, book a flight, cancel a booking, and list available flights.
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



