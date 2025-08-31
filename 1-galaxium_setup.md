# Set up the example `Galaxium Travels Infrastructure`

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

[Home](/README.md)