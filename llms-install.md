# Installing the VoiceMoat MCP server (for AI agents)

VoiceMoat is a hosted remote MCP server. There is nothing to clone, build or
run, and no API key to ask the user for.

- Transport: Streamable HTTP
- Address: `https://app.voicemoat.com/api/mcp`
- Authentication: OAuth. Unauthenticated requests get `401` with a
  `WWW-Authenticate` header pointing at the protected-resource metadata. The
  client opens the VoiceMoat sign-in page in the browser, the user signs in
  with Google, and the client stores the token.

## Steps

1. Add a remote MCP server named `voicemoat` with the address above. In a
   JSON settings file this is usually:

   ```json
   {
     "mcpServers": {
       "voicemoat": {
         "type": "streamableHttp",
         "url": "https://app.voicemoat.com/api/mcp"
       }
     }
   }
   ```

   Some clients spell the type `http` or take only `url`; use the client's
   own remote-server format.
2. Do not add an `Authorization` header. Sign-in happens through OAuth.
3. Ask the user to complete the VoiceMoat sign-in in the browser window that
   opens on the first tool call.
4. Check the connection by calling `get_me`. It returns the signed-in account
   and its plan.

## What the user needs

A VoiceMoat account on the Pro or Enterprise plan. Connecting works on any
plan, but each tool explains that the plan does not include it until the user
upgrades.

## Posting is never one call

`publish_post` and `schedule_post` return a preview and a one-time code on the
first call and post nothing. Only a second call with that code, made after the
user agrees, posts.
