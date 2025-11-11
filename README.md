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