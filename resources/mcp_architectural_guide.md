# Model Context Protocol (MCP) Architectural Guide

This comprehensive reference document breaks down the core architecture, operational mechanics, and integration loops of the Model Context Protocol (MCP). It is structured to help software architects and developers understand how MCP connects AI models with external tools, data resources, and development workflows.

---

## 📖 Core Glossary of Terms

To understand MCP and its integration into modern AI systems like GitHub Copilot (GHCP), you must first familiarize yourself with the standard terminology:

*   **MCP Client (The User / Brain):** The orchestration application that manages user conversations and connects to an LLM. It discovers capability schemas from connected servers, coordinates tool execution, and handles context routing. *Examples: VS Code, Claude Desktop, Cursor editor.*
*   **MCP Server (The Provider):** A lightweight application or runtime layer that exposes specific tools, dynamic data sources, or templates to an LLM. It does not contain generative logic; it waits for structured commands from a client.
*   **Tools:** Executable server-side functions with predefined JSON schemas that the AI model can dynamically invoke to perform external mutations or calculations (e.g., executing a local script, creating a database entry).
*   **Resources:** Static or dynamic context nodes identified by unique URIs that an AI model can read to pull files, system records, or real-time application states directly into its prompt window.
*   **Built-in Tools:** System-level capabilities provided directly by the host IDE/Client environment natively (e.g., standard text editors or internal workspace searching).
*   **Skills:** Aggregated behavior bundles that couple custom MCP servers with predefined runtime rules to solve specialized workflows.
*   **MCP Engine / SDK:** The foundational software dependency running inside both clients and servers that handles message parsing, validation, serialization, and connection lifecycle enforcement.

---

## ⚖️ MCP Client vs. MCP Server

The fundamental difference between an MCP Client and an MCP Server centers on **orchestration versus capability presentation**.

| Characteristic | MCP Client (The Orchestrator) | MCP Server (The Operator) |
| :--- | :--- | :--- |
| **Primary Responsibility** | Orchestrates workflows, connects to LLMs, handles UX. | Exposes tools, manages local context, streams raw data. |
| **Logic Mode** | Proactive (Asks the model what to do, routes the commands). | Reactive (Waits for incoming requests, returns inputs/outputs). |
| **Data Scope** | Session memory, model interaction history, global workspace settings. | Direct sandboxed system level utilities, databases, API endpoints. |
| **Integration Point** | Connects to downstream models and multiple external servers simultaneously. | Binds to explicit code functions or infrastructure platforms. |

---

## 🛠️ Minimal Technical Architecture Code Examples

### 1. Minimal MCP Server (Python via FastMCP)
The following code establishes an MCP Server containing an analytical tool and a telemetry data resource.

```python
from mcp.server.fastmcp import FastMCP

# Initialize the server instance
mcp = FastMCP("DeveloperServer")

@mcp.tool()
def calculate_sum(a: int, b: int) -> int:
    """Add two numbers together."""
    return a + b

@mcp.resource("info://status")
def get_status() -> str:
    """Return the current system status metrics."""
    return "System is running smoothly."

if __name__ == "__main__":
    mcp.run()
```

### 2. Minimal MCP Client Loop (Python Standard I/O)
This baseline client spawns the server as a background subprocess, initiates connection parameters, and runs automated discovery.

```python
import asyncio
from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client

async def run_client():
    server_params = StdioServerParameters(command="python", args=["server.py"])
    
    async with stdio_client(server_params) as (read_stream, write_stream):
        async with ClientSession(read_stream, write_stream) as session:
            await session.initialize()
            print("🚀 Client Session Initialized!")

            # Discovery phase
            tools_list = await session.list_tools()
            for tool in tools_list.tools:
                print(f"Discovered Capability: {tool.name}")
                
            # Execution phase
            result = await session.call_tool("calculate_sum", arguments={"a": 12, "b": 18})
            print(f"Execution Output: {result.content[0].text}")

if __name__ == "__main__":
    asyncio.run(run_client())
```

---

## 🔄 The AI Agent Loop (Client, LLM, and Server Orchestration)

When a user provides an action-oriented prompt, the Client acts as a transactional router between the LLM brain and the local MCP Server execution pipe.

```
┌──────┐              ┌──────────────┐              ┌────────────┐
│ User │              │  MCP Client  │              │    LLM     │
└────┬─┘              └──────┬───────┘              └─────┬──────┘
     │   Ask Question        │                            │
     │──────────────────────>│                            │
     │                       │    1. Send prompt + tools  │
     │                       │───────────────────────────>│
     │                       │                            │
     │                       │    2. Stop! "I need tool X"│
     │                       │<───────────────────────────│
     │                       │                            │
     │                       │ (Runs Tool on MCP Server)  │
     │                       │                            │
     │                       │    3. Send tool results    │
     │                       │───────────────────────────>│
     │                       │                            │
     │                       │    4. Final text answer    │
     │                       │<───────────────────────────│
     │   Deliver Answer      │                            │
     │<──────────────────────│                            │
```

### Operational Steps in detail:
1.  **Capability Schema Bundling:** The client aggregates all local and remote MCP tool definitions. It structures them alongside the active prompt and files, sending the complete payload to the downstream LLM.
2.  **Tool Selection Interception:** The LLM parses the payload, identifies that it requires missing data or outside access, halts standard text generation, and outputs an implicit `tool_use` metadata instruction.
3.  **Local Context Execution:** The client intercepts this tool call payload, runs the matching implementation inside the local or remote MCP server container, and intercepts the string data returned.
4.  **Final Generation Handshake:** The client injects the raw server response back into the conversation thread as a `tool_result` message context layer, allowing the LLM to process it and generate a complete text solution.

---

## 🌐 MCP Protocols vs. Traditional Infrastructure Frameworks

### Why traditional protocols like HTTP alone are insufficient for AI
Traditional web interactions rely entirely on standard network infrastructure (like HTTP) to move raw text data packages across servers. However, HTTP does not provide structural abstraction or predictable syntax mappings that an LLM can reason about natively. Every software platform constructs arbitrary JSON properties for their endpoints, leading to broken parsing logic when LLMs attempt functional automation.

### The Architectural Role of JSON-RPC 2.0 and MCP
MCP layers on top of standard networking stacks to enforce a rigid architectural taxonomy. It treats **JSON-RPC 2.0 as the grammar syntax layer** and **MCP as the context configuration schema layer**.

```
┌────────────────────────────────────────────────────────┐
│ APPLICATION LAYER: Model Context Protocol (MCP)       │ <-- Definitions (tools/list, resources/read)
├────────────────────────────────────────────────────────┤
│ SERIALIZATION LAYER: JSON-RPC 2.0                       │ <-- Frame Structures (method, params, jsonrpc)
├────────────────────────────────────────────────────────┤
│ TRANSPORT LAYER: Streamable HTTP (SSE) or Local stdio  │ <-- Data Pipe (POST requests or Process Pipes)
└────────────────────────────────────────────────────────┘
```

#### Network Packet Anatomy (Remote Web Execution Over HTTP):
When a client triggers an external remote tool over standard internet infrastructure, the underlying structural packet combines all three network layers:

```http
POST /mcp/v1/tools/call HTTP/1.1                 ◄── HTTP LAYER (Transport Routing)
Host: api.githubcopilot.com                       ◄── HTTP LAYER (Domain Resolution)
Authorization: Bearer gho_XYZ123...              ◄── HTTP LAYER (Security Context)
Content-Type: application/json                   
                                                 
{                                                
  "jsonrpc": "2.0",                              ◄── JSON-RPC LAYER (Grammar Format)
  "id": 105,                                      ◄── JSON-RPC LAYER (Session Tracking ID)
  "method": "tools/call",                        ◄── MCP LAYER (The AI Intent Spec)
  "params": {                                    ◄── MCP LAYER (Payload Mapping)
    "name": "fetch_github_issue",                
    "arguments": { "issue_number": 5 }           
  }                                              
}                                                
```

---

## 📑 Structural Breakdown: Tools vs. Resources

Inside the underlying protocol payloads, **Tools** and **Resources** use distinct schema structures to guide LLM text planning.

### 1. Tools (Action Mappings with Input Schemas)
Tools utilize strict JSON-schema formatting blocks to explicitly instruct the model on valid data types, property boundaries, and variable limits.

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "tools": [
      {
        "name": "calculate_factorial",
        "description": "Computes the factorial of a given integer.",
        "inputSchema": {
          "type": "object",
          "properties": {
            "n": { "type": "integer", "minimum": 0, "description": "Target number." }
          },
          "required": ["n"]
        }
      }
    ]
  }
}
```

### 2. Resources (Context Mappings with Static URIs)
Resources abandon complex dynamic parameters in favor of standard Uniform Resource Identifiers (URIs) and explicit MIME-types, forcing the model to access data exactly like an internal hyperlinked network asset.

```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "resources": [
      {
        "uri": "logs://system/today",
        "name": "Daily System Logs",
        "description": "Real-time application logs.",
        "mimeType": "text/plain"
      }
    ]
  }
}
```

---

## 🤝 The Stateful Initialization Handshake Lifecycle

Before an MCP connection transitions to an authenticated operational status, the client and server must execute a strict initial handshaking choreography over the connection stream.

```
1. Client Sends 'initialize' Request 
   ├── Validates Protocol compatibility version string
   └── Declares Client-side capabilities (e.g., roots, sampling)
   
2. Server Evaluates & Sends 'initialize' Result
   ├── Validates protocol version constraints
   └── Broadly declares its server details & capability blocks (e.g., tools, resources)
   
3. Client Transmits 'notifications/initialized' Message
   ├── Fired as a one-way notification (No transaction ID block)
   └── Unlocks the server state machine into 'connected' operational phase
```

### Sub-System Hooks for Initialization Lifespans
Modern development layers utilize this handshake milestone to spawn background system initialization frameworks. For instance, developers use `FastMCP` server-side context managers to hook directly into the finalized handshake and hydrate database pools or cache models before tools receive traffic:

```python
from contextlib import asynccontextmanager
from mcp.server.fastmcp import FastMCP

@asynccontextmanager
async def app_lifespan(server: FastMCP):
    # Triggers immediately after notifications/initialized passes validation
    print("Handshake established. Preloading system assets...")
    db_resources = {"pool": "active_connection"}
    yield {"db": db_resources}
    # Triggers on connection tear down
    print("Closing active infrastructure streams.")

mcp = FastMCP("DataStreamServer", lifespan=app_lifespan)
```

---

## 🔍 Engineering Traceability & Debugging Best Practices

### The Stdio Standard Output Conflict Rule
When building local MCP integrations via Standard Input/Output (`stdio`) pipes, **`stdout` is explicitly reserved by the system to route clean JSON-RPC strings**. 

*   **Anti-Pattern:** Inserting runtime validation logs or prints using standard instructions (`print()` in Python or `console.log()` in JS) outputs unstructured text directly into the client text buffer. This invalidates the JSON parsing logic, breaking the active connection immediately.
*   **Production Solution:** All debugging trace output, framework state logs, and diagnostic dumps must be explicitly written to the Standard Error (`stderr`) channel (via `sys.stderr.write()` or `console.error()`). The client workspace intercepts `stderr` data packages independently, printing them directly inside the specialized developer console output blocks without altering the JSON-RPC lifecycle stream.