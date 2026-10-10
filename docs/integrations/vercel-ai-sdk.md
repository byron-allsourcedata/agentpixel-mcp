# AgentPixel with the Vercel AI SDK

Install:

```bash
npm i ai @ai-sdk/mcp @ai-sdk/anthropic
```

```ts
import { generateText, stepCountIs } from 'ai';
import { anthropic } from '@ai-sdk/anthropic';
import { createMCPClient } from '@ai-sdk/mcp';

const headers: Record<string, string> = { 'X-AgentPixel-Source': 'vercel-ai-sdk' };
if (process.env.AGENTPIXEL_API_KEY) headers.Authorization = `Bearer ${process.env.AGENTPIXEL_API_KEY}`;

const agentpixel = await createMCPClient({
  transport: { type: 'http', url: 'https://agentpixel.io/mcp', headers },
});

try {
  const { text } = await generateText({
    model: anthropic('claude-sonnet-4-5'),
    tools: await agentpixel.tools(),
    stopWhen: stepCountIs(8),
    prompt: 'Show the AgentPixel setup status, then list consented contacts from today.',
  });
  console.log(text);
} finally {
  await agentpixel.close();
}
```

In a Next.js app, put this in a route handler or server action. Never ship the `px_` key to the browser. The only thing that goes into your pages is the script tag that `create_pixel` returns.
