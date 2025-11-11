## Setting up MCP Project

Following the official MCP documentation, you'll set up your own MCP project using uv for Python project management.
- 1. Open a terminal in VSCode if one is not already open (Terminal → New Terminal)
- 2. Create a new MCP project using uv
- 3. Add MCP dependencies with CLI tools
- 4. Verify your project structure
  
### Commands to run:

```
cd /home/lab-user
uv init flight-booking-server
cd flight-booking-server
uv add "mcp[cli]"
```

### Why these commands?

- uv init creates a proper Python project with pyproject.toml
- mcp[cli] includes both MCP SDK and development tools (MCP Inspector)
- This follows the official MCP development workflow

We've created a basic server structure with an airport data function. Your task is to make it an MCP Resource by adding the correct decorator.

- Open the server.py file in your project directory
- Find the get_airports() function
- Add the correct MCP resource decorator above it
- Resources provide read-only data access to AI systems
  
⚠️ Important: The resource must be defined with the URI scheme file://airports - MCP resources require proper URI schemes to be correctly identified by AI systems.


🦘 Connect Your MCP Server to Roo-Code
Now let's test your MCP server by connecting it to the Roo-Code AI assistant!

🔧 Configure MCP Server:

- 1.Look for the roo-code plugin icon (kangaroo logo 🦘) in the left sidebar
- 2.Click on "MCP Servers" at the top (stacked servers icon 📚)
- 3.Click "Edit Project MCP" button
- 4.Replace the file contents with this MCP configuration:

```  
{
  "mcpServers": {
    "flight-booking": {
      "command": "uv",
      "args": ["run", "python", "server.py"],
      "cwd": "/home/lab-user/flight-booking-server"
    }
  }
}

```

🔄 Activate and Test
- 5.Save the file and click "Refresh MCP Servers" button
- 6.Verify that "flight-booking" appears in the server list
- 7.Click "Done" to return to the chat interface
- 8.Test your MCP server with these commands:
  
```  
"Search for flights from LAX to JFK using the flight-booking server"
"Get airport information using the flight-booking MCP server"
"Book flight FL123 for John Doe using flight-booking server"
```  

✅ MCP Server Complete!
You've successfully built a complete MCP server following official patterns with:

✅ Proper project setup using uv and official MCP package
✅ Resources for airport information
✅ Tools for flight search and booking
✅ Prompts for AI guidance
✅ Testing with official MCP development tools
✅ Integration with Roo-Code AI assistant

🚀 What you've learned:

- Official MCP development workflow with uv
- FastMCP framework from mcp.server.fastmcp
- Resource, Tool, and Prompt implementation patterns
- MCP Inspector for server testing and validation
- Real-world MCP server integration with AI assistants

🔜 Next Steps:

- Explore HTTP transport: mcp.run(transport="http")
- Add error handling and input validation
- Integrate with real flight APIs

-----------------------------------------------------------------------------------------------------

🤖 **MCP Client Development**
In this lab, you'll learn to build Python clients that interact with MCP servers programmatically. Move beyond the Inspector to create custom automation and integration solutions.

What you'll build:

1. Basic connection clients for server discovery
2. Tool execution clients for automation
3. Advanced clients with roots, sampling, and elicitation
4. Production-ready integration patterns

🌐 Cloud Environment: You'll develop Python clients and connect them to MCP servers via HTTP transport.

Step 1: Start the MCP Server

Before we can test MCP clients, we need to start the flight booking server that our clients will connect to.

1. Open a terminal and navigate to the server directory
2. Start the MCP server using the MCP CLI with streamable-http transport
3. Verify the server is running on port 8000
4. Leave this terminal open - the server must keep running
5. Commands to run:

```
cd /home/lab-user/flight-booking-server
uv run mcp run server.py --transport streamable-http

```
⚠️ Important:

* Server runs on 127.0.0.1:8000 (MCP CLI default)
* You should see "Server running on 127.0.0.1:8000" when server starts
* All clients are configured to connect to http://localhost:8000/mcp/
* Keep this terminal open throughout the lab!

## Server Resource Discovery

Run the basic client to discover what resources the flight booking server provides. The server offers multiple resources that clients can access.

🔍 **Discovery Task:**

1. Open a new terminal (keep the server terminal running!)
2. Navigate to the mcp-client project directory
3. Run the basic client to see all available resources
4. Identify the additional resource beyond the airports data
5. Command to run:

```
cd /home/lab-user/mcp-client
uv run python basic_client.py

```


💡 Look for the "Available Resources" section in the output!

### Output 

```
🔌 BASIC MCP CLIENT
========================================
🎯 Goal: Connect to flight booking server and discover capabilities
========================================
✅ Connected to MCP server successfully!

🔧 Available Tools:
  - search_flights: Search for flights between two airports
  - create_booking: Create a flight booking

📊 Available Resources:
  - file://airports/: get_airports
  - file://airlines/: get_airlines

💬 Available Prompts:
  - find_best_flight: Generate a prompt for finding the best flight within budget
  - handle_disruption: Generate a prompt for handling flight disruptions

🎉 Basic client connection successful!
✨ Server capabilities discovered successfully!

```
-------------------------------------------------------------------------------------------

## Tool Parameter Analysis

Examine the tools_client.py file to understand how MCP clients call server tools with specific parameters.

📋 Analysis Task:

1. Open and examine the file: /home/lab-user/mcp-client/tools_client.py
2. Look at the search_flights tool call in Test 1
3. Identify the destination airport parameter value
4. Optionally run the client to see the tools in action

🔍 Code Location:

```
flight_result = await client.call_tool("search_flights", {
    "origin": "LAX",
    "destination": "???"
})
```

Command to test:

```
cd /home/lab-user/mcp-client
uv run python tools_client.py
```

### Output

```
mcp-client on  master [?] ➜  uv run python tools_client.py 
🛠️ TOOLS & PROMPTS CLIENT
========================================
🎯 Goal: Test flight booking server tools and prompts
========================================
✅ Connected to flight booking server!

✈️ Test 1: Searching for flights...
🎯 Flight search result:
   {
  "flights": [
    {
      "id": "FL123",
      "origin": "LAX",
      "destination": "JFK",
      "price": 299
    },
    {
      "id": "FL456",
      "origin": "LAX",
      "destination": "JFK",
      "price": 399
    }
  ]
}

🎫 Test 2: Creating a booking...
🎯 Booking result:
   {
  "booking_id": "BK123",
  "flight_id": "FL123",
  "passenger": "Alice Johnson",
  "status": "confirmed"
}

🏢 Test 3: Getting airport information...
🎯 Airport information:
   {
  "LAX": {
    "name": "Los Angeles International",
    "city": "Los Angeles"
  },
  "JFK": {
    "name": "John F. Kennedy International",
    "city": "New York"
  },
  "LHR": {
    "name": "London ...

💡 Test 4: Getting flight recommendations...
🎯 Flight recommendation prompt:
   Please help me find the best flight within a $500.0 budget.
    
My preferences: economy class, direct flight

Please consider:
- Price (must be under $500.0)
- Flight duration  
- Airline reputation
- Departure times

Use the search_flights tool to find available options and provide a recommendatio...

🚨 Test 5: Getting disruption handling prompt...
🎯 Disruption handling prompt:
   A passenger's flight FL123 has been disrupted due to: weather delay

Please help resolve this by:
1. Understanding the passenger's situation
2. Finding alternative flight options using search_flights
3. Providing clear rebooking steps
4. Offering appropriate compensation if applicable

Be empathetic...

🎉 All tools and prompts tested successfully!
✨ Flight booking server is fully functional!
```

## MCP Roots Configuration

Examine the roots_client.py file to understand which directories are provided as file system roots to the server.

📋 Analysis Task:

1. Open and examine the file: /home/lab-user/mcp-client/roots_client.py
2. Look at the project_roots list defined at the top
3. Identify which directory path is NOT included in the current roots
4. Optionally run the client to see roots functionality

🔍 Code Location:

```
project_roots = [
    "file:///home/lab-user/",
    "file:///home/lab-user/flight-booking-server/",
    "file:///home/lab-user/mcp-client/"
]
```

Command to test:

```
cd /home/lab-user/mcp-client
uv run python roots_client.py
```

### Output

```
mcp-client on  master [?] ➜  uv run python roots_client.py 
📁 ROOTS MCP CLIENT
========================================
🎯 Goal: Provide file system access to server via roots
========================================
✅ Connected to server with roots support!

🔧 Available tools:
  - search_flights: Search for flights between two airports
  - create_booking: Create a flight booking

📊 Available resources:
  - file://airports/: get_airports
  - file://airlines/: get_airlines

📄 Testing file access capabilities...
✅ Successfully accessed airports resource
   Content length: 237 characters

🌳 Roots configuration summary:
📁 Provided 3 project roots:
   1. file:///home/lab-user/
   2. file:///home/lab-user/flight-booking-server/
   3. file:///home/lab-user/mcp-client/

💡 The server can now access files within these directories
   if it has file-related tools implemented!

🎉 Roots functionality test completed!
✨ Server now has potential access to specified directories!
```

## Implementing Sampling Support

Sampling allows servers to request LLM responses from clients. Learn how to handle these requests.

1. Examine the sampling_client.py file
2. Run it to see the sampling callback in action
3. Understand how to handle CreateMessageRequestParams
4. See how to return CreateMessageResult responses

Command to run:

```
cd /home/lab-user/mcp-client
uv run python sampling_client.py
```

### Output 

```
mcp-client on  master [?] ➜  uv run python sampling_client.py 
🎭 SAMPLING MCP CLIENT
========================================
🎯 Goal: Handle server LLM sampling requests
========================================
✅ Connected to server with sampling support!

🔧 Checking for sampling-enabled tools...
  📝 create_booking: Create a flight booking

🧪 Testing potential sampling scenarios...
💡 Testing flight recommendation prompt...
✅ Prompt generated successfully (no sampling required)

🎭 Sampling callback is ready!
   If the server had tools that use ctx.session.create_message(),
   our sampling callback would handle those requests.

📋 Sampling callback features:
   - Handles travel explanation requests
   - Provides travel/flight explanations
   - Creates stories and recommendations
   - Returns contextual responses

🎉 Sampling client test completed!
✨ Ready to handle server LLM requests!
```

## Implementing Interactive Elicitation

Elicitation allows servers to request user input from clients. Experience true interactive MCP communication where the server can ask you for information directly.

1. Examine the elicitation_client.py file
2. Run it to see the interactive elicitation callback
3. Understand how real user input is captured
4. Experience live server-to-user communication

Command to run:

```
cd /home/lab-user/mcp-client
uv run python elicitation_client.py
```

### Output

```
mcp-client on  master [?] ➜  uv run python elicitation_client.py 
🔔 ELICITATION MCP CLIENT
========================================
🎯 Goal: Handle server user input requests
========================================
✅ Connected to server with elicitation support!

🔧 Checking for interactive tools...
  ⚠️  No obvious interactive tools found
     This is normal - the basic flight server doesn't have user input tools

🧪 Testing for potential elicitation triggers...
🎫 Testing booking creation (might ask for user details)...
✅ Booking result: {
  "booking_id": "BK456",
  "flight_id": "FL456",
  "passenger": "Test User",
  "status": "confirmed"
}

🔔 Elicitation callback is ready!
   If the server had tools that use ctx.session.elicit(),
   our elicitation callback would handle those requests.

📋 Elicitation callback features:
   - Prompts user for real input when server requests
   - Handles text responses and JSON input
   - Intelligently parses responses based on request type
   - Allows user to decline with Ctrl+C
   - Supports various input formats

🎯 Example user interaction:
   • Server asks: 'Please enter your name'
   • Client prompts: 'Please enter your response: '
   • User types: 'John Smith'
   • Client sends: {'name': 'John Smith'}

💡 Supported input formats:
   • Simple text: 'John Smith' → {'name': 'John Smith'}
   • JSON: '{"name": "John", "age": 30}' → parsed as JSON
   • Numbers: '500' for budget → {'budget': 500.0, 'currency': 'USD'}
   • Confirmations: 'yes' → {'confirmation': 'yes'}

🎉 Elicitation client test completed!
✨ Ready to handle server user input requests!
```

## Complete MCP Client Implementation

Now let's test a complete client that combines all MCP capabilities in one comprehensive implementation.

1. Examine the complete_client.py file
2. Run it to see all features working together
3. Observe the phased testing approach
4. See how all callbacks work in harmony

Command to run:

```
cd /home/lab-user/mcp-client
uv run python complete_client.py
```

### Output

```
mcp-client on  master [?] ➜  uv run python complete_client.py 
🌟 COMPLETE MCP CLIENT
==================================================
🎯 Goal: Demonstrate full MCP client capabilities
==================================================
✅ Connected with full MCP capabilities!

🔍 PHASE 1: Discovery
------------------------------
🔧 Found 2 tools:
  • search_flights: Search for flights between two airports
  • create_booking: Create a flight booking

📊 Found 2 resources:
  • file://airports/
  • file://airlines/

💬 Found 2 prompts:
  • find_best_flight: Generate a prompt for finding the best flight within budget
  • handle_disruption: Generate a prompt for handling flight disruptions

🛠️ PHASE 2: Tools Testing
------------------------------
✈️ Testing flight search...
   ✅ Result: {
  "flights": [
    {
      "id": "FL123",
      "origin": "SFO",
      "destination": "NYC",
      "price": 299
    },
    {
      "id": "FL456",
      "origin": "SFO",
      "destination": "NYC",
      "price": 399
    }
  ]
}

🎫 Testing booking creation...
   ✅ Result: {
  "booking_id": "BK789",
  "flight_id": "UA789",
  "passenger": "Sarah Wilson",
  "status": "confirmed"
}

📊 PHASE 3: Resources Testing
------------------------------
🏢 Testing airport resource...
   ✅ Retrieved 237 characters of airport data

💡 PHASE 4: Prompts Testing
------------------------------
🎯 Testing flight recommendation prompt...
   ✅ Prompt generated successfully
   📝 Message count: 1

🚨 Testing disruption handling prompt...
   ✅ Disruption prompt generated successfully

🎊 PHASE 5: Test Summary
------------------------------
✅ Client Capabilities Demonstrated:
   🔌 Basic connection and initialization
   🔧 Tool discovery and execution
   📊 Resource access and reading
   💬 Prompt generation and parameterization
   📁 Project roots provision
   🤖 LLM sampling response handling
   🔔 User input elicitation handling

🌟 Complete MCP client implementation successful!
✨ Ready for production use and integration!
```


 # **Building Production MCP Clients**

 Learn the key considerations for building robust, production-ready MCP clients.

1. Error handling and connection retry logic
2. Proper async/await patterns and resource cleanup
3. Security considerations for roots and callbacks
4. Testing strategies for MCP client applications

🏗️ Best practices covered:

* Connection management and error recovery
* Callback implementation patterns
* Integration with existing Python applications
* Monitoring and logging for MCP operations