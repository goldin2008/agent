# Agent

AI-powered agent system leveraging AWS infrastructure, Model Context Protocol (MCP), and Claude AI for intelligent task execution with external API integration.

> https://github.com/WaelDataReply/AmazonAgentCore_GW_demo

> https://medium.com/@wael-saideni/building-production-ready-mcp-workflows-with-amazon-bedrock-agentcore-gateway-d8386db65df3


---

## 📋 Table of Contents
- [System Overview](#system-overview)
- [Architecture Components](#architecture-components)
- [Operational Flow](#operational-flow)
- [Security & Authentication](#security--authentication)

---

## System Overview

This solution implements an intelligent AI agent that:
- Processes user requests through a conversational interface
- Makes autonomous decisions to invoke external tools
- Securely integrates with third-party APIs
- Manages authentication across multiple architectural layers

![Agent Architecture](./image/1__Py40WydkLrMSai17DE63Q.webp)

---

## Architecture Components

### User Interface & Application Layer
| Component | Description |
|-----------|-------------|
| **User Interface** | Sends requests and receives responses from the AI agent |
| **EC2 Instance** | Hosts the MCP client application running the Strands-based AI agent |
| **MCP Client-Strands Agent** | Agent implementation using AWS Strands framework (v1.0.1), framework-agnostic |

### AI Processing Layer
| Component | Description |
|-----------|-------------|
| **Amazon Bedrock** | Hosts Claude Sonnet 3.5 model providing language model capabilities |
| **Claude Sonnet 3.5** | Large language model powering intelligent agent responses |

### MCP Infrastructure
| Component | Description |
|-----------|-------------|
| **AgentCore Gateway** | Transforms OpenAPI specifications into MCP-compliant tools |
| **AgentCore Identity** | Manages comprehensive authentication and authorization |

### Security & Identity Management
| Component | Description |
|-----------|-------------|
| **Amazon Cognito** | Handles end-user authentication and token management |
| **OAuth 2.0 Protocols** | Implements secure authentication across architectural layers |

### External Integration
| Component | Description |
|-----------|-------------|
| **Weather API** | External REST API providing Mars weather data |
| **MCP Tool** | Bridges Weather API with agent via MCP specification |

---

## Operational Flow

### 1. User Request Flow
```
User Query
    ↓
EC2 Application (Strands Agent)
    ↓
Agent Evaluates: Tool Needed?
    ↓
[Yes] → Route to AgentCore Gateway
[No]  → Direct LLM Response
```

**Steps:**
1. User sends a query to the application
2. EC2 application receives and processes the request
3. Agent determines if external tool invocation is needed

### 2. Language Model Interaction
```
Agent Request
    ↓
Amazon Bedrock
    ↓
Claude Sonnet 3.5
    ↓
Response or Tool Decision
```

**Steps:**
1. Agent invokes Claude Sonnet 3.5 via Amazon Bedrock
2. Model generates response or determines tool need
3. Response routed back to agent

### 3. Tool Usage via AgentCore Gateway
```
Agent ← JWT Token → AgentCore Gateway
                ↓
          Translate MCP → REST
                ↓
          Weather API
                ↓
        Return Data
```

**Steps:**
1. Agent communicates with AgentCore Gateway via streamable HTTP
2. Agent lists available tools or invokes specific tool
3. Inbound authentication uses JWT OAuth tokens
4. Gateway translates MCP request into REST API call
5. Results returned to agent

---

## Security & Authentication

### Authentication Layers

| Layer | Method | Purpose |
|-------|--------|---------|
| **Inbound (Agent → Gateway)** | JWT OAuth Token | Secures agent-to-gateway communications |
| **Outbound (Gateway → Weather API)** | API Key / OAuth Token | Secures third-party API access |
| **User Authentication** | OAuth 2.0 Ingress (RFC 7591) | Manages end-user access via Cognito |
| **Tool Authentication** | OAuth 2.0 Egress (RFC 9728) | Manages tool-level authorization |

### Key Security Components
- **Amazon Cognito**: User authentication and token management
- **AgentCore Identity**: Tool-level authentication and authorization
- **JWT Tokens**: Secure communication between system components
- **OAuth 2.0 Protocols**: Multi-layer secure authentication

