---
title: 'Building a WhatsApp MCP Server: LLM Agents That Can Text'
date: 2025-04-22
draft: false
description: 'How I built whatsapp-mcp — a Model Context Protocol server that lets Claude and other LLMs send messages, read chats, and interact with WhatsApp.'
tags: ['mcp', 'llm', 'whatsapp', 'go', 'ai-agents']
categories: ['engineering']
---

[Model Context Protocol](https://modelcontextprotocol.io/) (MCP) is Anthropic's open standard for connecting LLMs to external tools and data sources. Instead of building a custom tool-calling integration for every model, you build one MCP server and any compatible client — Claude, Cursor, Zed, your own agent — can use it.

I built [`whatsapp-mcp`](https://github.com/felipeadeildo/whatsapp-mcp): an MCP server that gives LLM agents full access to WhatsApp. This post covers the architecture and the interesting problems I hit.

## What It Does

```
Claude → MCP Client → whatsapp-mcp server → WhatsApp Web (via whatsmeow)
```

The server exposes tools like:

- `send_message(to, text)` — send a message to a contact or group
- `list_chats()` — get recent conversations
- `read_messages(chat_id, limit)` — fetch messages from a chat
- `search_contacts(query)` — find contacts by name or number

Claude can then do things like: _"Send a message to João saying I'll be 10 minutes late"_ or _"Summarize my unread messages and tell me which ones need a reply."_

## The Protocol

MCP uses JSON-RPC 2.0 over stdio (or HTTP+SSE). A server declares its capabilities — tools, resources, prompts — and clients discover them at startup.

```go
// Tool definition (simplified)
tools := []mcp.Tool{
    {
        Name: "send_message",
        Description: "Send a WhatsApp message to a contact or group",
        InputSchema: mcp.Schema{
            Type: "object",
            Properties: map[string]mcp.Property{
                "to":   {Type: "string", Description: "Phone number or JID"},
                "text": {Type: "string", Description: "Message content"},
            },
            Required: []string{"to", "text"},
        },
    },
}
```

When Claude decides to call `send_message`, it sends:

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "send_message",
    "arguments": { "to": "5511999998888", "text": "Chego em 10 minutos" }
  }
}
```

The server handles it, calls WhatsApp, returns the result.

## WhatsApp Integration

I used [`whatsmeow`](https://github.com/tulir/whatsmeow) — a Go library that implements the WhatsApp Web multi-device protocol. It handles:

- QR code authentication
- Persistent session storage (SQLite)
- Message sending and receiving
- Media handling

The first-time setup requires scanning a QR code to link your phone. After that, the session is stored and the server reconnects automatically.

```go
client := whatsmeow.NewClient(deviceStore, waLog.Stdout("Client", "DEBUG", true))

if client.Store.ID == nil {
    // First login — show QR code
    qrChan, _ := client.GetQRChannel(context.Background())
    go func() {
        for evt := range qrChan {
            if evt.Event == "code" {
                qrterminal.Generate(evt.Code, qrterminal.L, os.Stdout)
            }
        }
    }()
    client.Connect()
} else {
    client.Connect()
}
```

## The Interesting Problems

**Message JIDs.** WhatsApp uses JIDs (Jabber IDs) to identify users: `5511999998888@s.whatsapp.net` for individuals, `groupid@g.us` for groups. When Claude says "message João", the server has to resolve the name to a JID. I added a contacts cache that syncs on startup.

**Rate limiting.** WhatsApp will temporarily ban numbers that send messages too fast. I added a token bucket rate limiter — max 10 messages/minute for individual chats, stricter for groups.

**Context length.** Returning full chat history to the LLM is expensive. I added a `limit` parameter and message summarization — the tool can return either raw messages or a pre-summarized view.

## Using It with Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "whatsapp": {
      "command": "/path/to/whatsapp-mcp",
      "args": []
    }
  }
}
```

Restart Claude Desktop, and you'll see WhatsApp tools available. First run shows the QR code in the terminal — scan it with your phone.

## What's Next

- Media support (send images, read image messages)
- Better contact resolution (fuzzy name matching)
- Scheduled messages
- Reaction support

The repo is at [github.com/felipeadeildo/whatsapp-mcp](https://github.com/felipeadeildo/whatsapp-mcp) — PRs welcome.
