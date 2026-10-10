# AgentPixel with LangChain and LangGraph

Install:

```bash
pip install langchain-mcp-adapters langgraph "langchain[anthropic]"
```

```python
import asyncio, os
from langchain_mcp_adapters.client import MultiServerMCPClient
from langgraph.prebuilt import create_react_agent

headers = {"X-AgentPixel-Source": "langchain"}
if os.getenv("AGENTPIXEL_API_KEY"):
    headers["Authorization"] = f"Bearer {os.environ['AGENTPIXEL_API_KEY']}"

client = MultiServerMCPClient({
    "agentpixel": {
        "transport": "streamable_http",
        "url": "https://agentpixel.io/mcp",
        "headers": headers,
    }
})

async def main():
    tools = await client.get_tools()
    agent = create_react_agent("anthropic:claude-sonnet-4-5", tools)
    result = await agent.ainvoke({"messages": [
        {"role": "user", "content": "Create a pixel for https://example.com, configure consent, and give me the script tag."}
    ]})
    print(result["messages"][-1].content)

asyncio.run(main())
```

Without `AGENTPIXEL_API_KEY`, ask the agent to "start an AgentPixel trial for me@example.com" first. It calls `signup_free`, you confirm the email, and it calls `confirm_signup` and `create_api_key`.

Newer releases of `langchain-mcp-adapters` also accept `"transport": "http"`. Use the model you already have configured.
