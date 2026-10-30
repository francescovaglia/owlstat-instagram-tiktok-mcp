# Installing the OwlStat MCP server

OwlStat is a hosted remote server: there is nothing to clone, build or install.

1. Ask the user for their OwlStat API key. They find it in their OwlStat settings, API section (https://owlstat.io). If they have no account, they can create one for free at https://owlstat.io.
2. Add a remote MCP server named `owlstat`:
   - URL: `https://mcp.owlstat.io/mcp`
   - Transport: Streamable HTTP
   - Header: `Authorization: Bearer <API key>`
3. Restart the client if it does not reload servers by itself, then call `list_monitored_accounts` to check the connection. A 401 means the key is wrong or has a stray space.
