# figma-to-react-poc-agents

## Figma handoff reference files

- **UI Design — User Profile:** [Open in Figma](https://www.figma.com/design/Wfl80ma3KHVv9velWqou5V/Free-Figma-Admin-Dashboard-UI-Kit---Tailwind-Admin--Community-?node-id=1-4)
- **Design System:** [Open in Figma](https://www.figma.com/design/Wfl80ma3KHVv9velWqou5V/Free-Figma-Admin-Dashboard-UI-Kit---Tailwind-Admin--Community-?node-id=0-1)

## Figma MCP server configuration

Add this server configuration to your MCP client configuration to connect to Figma:

```json
{
  "inputs": [],
  "servers": {
    "figma": {
      "url": "https://mcp.figma.com/mcp",
      "type": "http"
    }
  }
}
```