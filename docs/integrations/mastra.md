# AgentPixel with Mastra

Install:

```bash
npm i @mastra/core @mastra/mcp @ai-sdk/anthropic
```

```ts
import { Agent } from '@mastra/core/agent';
import { MCPClient } from '@mastra/mcp';
import { anthropic } from '@ai-sdk/anthropic';

const headers: Record<string, string> = { 'X-AgentPixel-Source': 'mastra' };
if (process.env.AGENTPIXEL_API_KEY) headers.Authorization = `Bearer ${process.env.AGENTPIXEL_API_KEY}`;

export const mcp = new MCPClient({
  servers: {
    agentpixel: {
      url: new URL('https://agentpixel.io/mcp'),
      requestInit: { headers },
    },
  },
});

export const growthAgent = new Agent({
  name: 'growth-agent',
  instructions: 'You install and monitor the AgentPixel consent pixel and report contacts.',
  model: anthropic('claude-sonnet-4-5'),
  tools: await mcp.getTools(),
});

const res = await growthAgent.generate('Run check_install for my pixel and summarize the result.');
console.log(res.text);
```
