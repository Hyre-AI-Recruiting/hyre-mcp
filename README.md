# Hyre MCP server

[Hyre](https://hyre.io) is the AI layer over the ATS of a recruitment agency. The Hyre MCP server
puts your ATS and CRM, your email, LinkedIn and WhatsApp conversations, your calls, meetings,
agenda and tasks into Claude and other MCP clients.

This repository holds this README and [`server.json`](./server.json), the entry of the server in
the official MCP registry. Hyre hosts the server. You connect to it; you do not install anything.

|           |                                            |
| --------- | ------------------------------------------ |
| URL       | `https://mcp.hyre.io/mcp`                  |
| Transport | Streamable HTTP                            |
| Login     | OAuth 2.0 with your Hyre account           |
| Registry  | `io.hyre/mcp` in the official MCP registry |

You need a Hyre account. The tools work on the data of your own Hyre workspace and on the accounts
you connected to it (your ATS or CRM, Gmail or Outlook, LinkedIn, WhatsApp, your calendar).

## Connect

### Claude (claude.ai, desktop, mobile)

One connector covers claude.ai, the Claude desktop app and Claude mobile.

1. In Claude, go to **Customize → Connectors**.
2. Click **+**, then **Add custom connector**.
3. Paste `https://mcp.hyre.io/mcp`. Leave Advanced settings empty.
4. Click **Add**, then **Connect**, and log in with your Hyre account.
5. In a chat, open **+ → Connectors** and switch Hyre on.

On a Claude Team or Enterprise plan, only an Owner can add the connector, under **Organization
settings → Connectors**. Everyone else then connects to it.

Claude asks for permission the first time each tool runs. Click **Always allow** for the tools you
trust; keep the tools that write or send on approval. When Hyre changes a tool, Claude asks again
for that tool. Hyre changes an existing tool at most once a month.

### Claude Code

```bash
claude mcp add --transport http hyre https://mcp.hyre.io/mcp
```

Then run `/mcp` in Claude Code and log in.

### Other MCP clients

Add a remote server with the URL `https://mcp.hyre.io/mcp` and the Streamable HTTP transport. The
client finds the login through OAuth discovery and registers itself (dynamic client
registration, PKCE).

## Supported ATSs and CRMs

Hyre tests twelve ATSs end to end before every release: Bullhorn, Byner, Carerix, Connexys, Loxo,
MySolution, OTYS, RecruitCRM, Recruitee, Salesforce, Spott and Vincere. The CRMs HubSpot and Attio
are tested the same way. Other ATSs connect through a generic integration, with fewer tested reads
and writes.

## Tools

Your own Hyre routines and agents (for example "Start je dag") also appear as tools of their own.
They differ per workspace, so this list leaves them out.

**Start here**

| Tool                         | What it does                                                                 |
| ---------------------------- | ---------------------------------------------------------------------------- |
| `hyre_recruitment_dashboard` | Who is signed in, what is connected, and the routines and agents you can run |
| `find_contact`               | Find a candidate or contact in Hyre                                          |
| `find_conversation`          | Find your conversation with a person across email, LinkedIn and WhatsApp     |

**ATS and CRM**

| Tool             | What it does                                                              |
| ---------------- | ------------------------------------------------------------------------- |
| `ats_read`       | Read candidates, jobs, applications, companies and notes in your ATS      |
| `ats_write`      | Create and update records in your ATS                                     |
| `crm_read`       | Read contacts, companies and deals in your CRM                            |
| `crm_write`      | Create and update records in your CRM                                     |
| `log_activity`   | Log a call, meeting or message to the ATS or CRM                          |
| `documents`      | List, read and delete a record's documents, such as a candidate's CV      |
| `matching`       | Match candidates to a vacancy, or vacancies to a candidate, from your ATS |
| `merge_contacts` | Join two Hyre profiles of the same person                                 |
| `find_phone`     | Look up a phone number for a contact (uses enrichment credits)            |

**Conversations**

| Tool                  | What it does                                                  |
| --------------------- | ------------------------------------------------------------- |
| `emails`              | Search and read your email with candidates and clients        |
| `linkedin_messages`   | Read your LinkedIn conversations                              |
| `linkedin_profiles`   | Look up a LinkedIn profile                                    |
| `linkedin_send`       | Send a LinkedIn connection request or message                 |
| `whatsapp_messages`   | Read your WhatsApp conversations                              |
| `meetings`            | Read your recorded meetings, transcripts and summaries        |
| `phone_calls`         | Read your phone calls and their transcripts                   |
| `drafts`              | Draft a reply or a new message on email, LinkedIn or WhatsApp |
| `tone_of_voice`       | Read your tone of voice for messages                          |
| `tone_of_voice_write` | Save your tone of voice                                       |

**Agenda and tasks**

| Tool             | What it does                                            |
| ---------------- | ------------------------------------------------------- |
| `calendar`       | Read your calendar                                      |
| `calendar_write` | Create and change calendar events                       |
| `availability`   | Find free slots for an interview, intake or client call |
| `tasks`          | Read your follow-ups and to-dos                         |
| `tasks_write`    | Create and update tasks                                 |

**Sourcing and outreach**

| Tool             | What it does                                              |
| ---------------- | --------------------------------------------------------- |
| `sourcing`       | Plan and read a LinkedIn sourcing run for a vacancy       |
| `sourcing_write` | Run a sourcing run: fetch and score profiles, save a list |
| `outreach`       | Read a LinkedIn outreach campaign and preview it          |
| `outreach_write` | Create or update a LinkedIn outreach campaign or list     |

**Documents, web and agents**

| Tool              | What it does                                                          |
| ----------------- | --------------------------------------------------------------------- |
| `create_document` | Make a Word file and a PDF: a CV in house style, a candidate proposal |
| `read_document`   | Read a PDF or Word document                                           |
| `web`             | Search the web and read a page                                        |
| `get_agents`      | List and load your Hyre agents, routines and actions                  |
| `create_agent`    | Create a Hyre agent                                                   |
| `update_agent`    | Change a Hyre agent                                                   |
| `feedback`        | Send Hyre a short report on how a task went                           |

## Example prompts

- "Prepare me for my 14:00 client meeting." (`calendar`, `find_contact`, `crm_read`, `emails`)
- "Which candidates in our ATS fit the warehouse lead vacancy? Give me a shortlist." (`matching`)
- "Has the candidate replied to my last email? Draft a follow-up in my tone of voice." (`emails`,
  `tone_of_voice`, `drafts`)
- "Log my call with this candidate to the ATS." (`phone_calls`, `log_activity`)
- "Make a CV in our house style from this candidate's CV in the ATS." (`documents`,
  `create_document`)
- "When am I free this week for an intake?" (`availability`)
- "What do I have to follow up today?" (`tasks`)

## Security and data

- The server reads and writes only the data of the signed-in user's own workspace and connections.
- Tools that write or send are marked as such (`readOnlyHint: false`), so your client can ask you
  before each call.
- Hyre does not read your Claude memory, chat history or files.

## Support

Questions or problems: [hyre.io](https://hyre.io).
