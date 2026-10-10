# AgentPixel with CrewAI

Install:

```bash
pip install "crewai-tools[mcp]"
```

```python
import os
from crewai import Agent, Task, Crew
from crewai_tools import MCPServerAdapter

headers = {"X-AgentPixel-Source": "crewai"}
if os.getenv("AGENTPIXEL_API_KEY"):
    headers["Authorization"] = f"Bearer {os.environ['AGENTPIXEL_API_KEY']}"

server_params = {
    "url": "https://agentpixel.io/mcp",
    "transport": "streamable-http",
    "headers": headers,
}

with MCPServerAdapter(server_params) as tools:
    marketer = Agent(
        role="Website growth agent",
        goal="Install a consent-first pixel and report consented contacts",
        backstory="You manage the company website and its visitor consent.",
        tools=tools,
    )
    task = Task(
        description="Check setup_status. If no pixel exists for https://example.com, create one, configure consent and return the script tag. Then list today's consented contacts.",
        expected_output="The script tag, the install status and a contact count.",
        agent=marketer,
    )
    print(Crew(agents=[marketer], tasks=[task]).kickoff())
```

If your CrewAI version ignores `headers` in `MCPServerAdapter`, use the `mcps=[MCPServerHTTP(url=..., headers=...)]` field on `Agent` (from `crewai.mcp`).
