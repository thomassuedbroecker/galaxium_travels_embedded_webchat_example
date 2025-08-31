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

## The steps in the related [YouTube video: TBD]().

#### 1. Start your container engine

Ensure your container engine is running on your desktop.

#### 2. [Set up the example `Galaxium Travels Infrastructure`](https://github.com/thomassuedbroecker/galaxium_travels_embedded_webchat_example/blob/main/1-galaxium_setup.md)

#### 3. [Set up the watsons Orchestrate ADK and watsonx Orchestrate Development Server](https://github.com/thomassuedbroecker/galaxium_travels_embedded_webchat_example/blob/main/2-watsonx_adk_setup.md)

#### 4. [Generate a new Agent](https://github.com/thomassuedbroecker/galaxium_travels_embedded_webchat_example/blob/main/3-create_an_agent.md)

#### 5. Verify the Agent works

* Default Agent Architecture

Ask the following questions and see how the Agent responds to your question.

Question: `I want to book a flight?`

#### 6. Verify the agent behavior in Langfuse

Now we can inspect the detailed behavior of the Agent in [langfuse locally](http://localhost:3010). (orchestrate@ibm.com/orchestrate)

#### 7. [Add the Agent to the "Galaxium Travels" webapp](https://github.com/thomassuedbroecker/galaxium_travels_embedded_webchat_example/blob/main/4-embed_webchat.md)
