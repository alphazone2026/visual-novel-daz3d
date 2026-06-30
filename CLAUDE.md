# Visual Novel — Daz3D + AI Project

## Project Goal

Create a realistic-style visual novel game using Daz3D for 3D character and environment rendering. The workflow uses Claude (via MCP) to control Daz Studio directly — loading scenes, placing characters, posing them, and triggering renders — so finished images can be dropped into the visual novel engine.

Key requirements:
- Realistic character style (not anime)
- Consistent characters across all scenes (same figure, clothing, skin)
- Interior environments (house rooms — living room, bedroom, kitchen, etc.)
- Multiple poses and positions per scene
- AI-controlled rendering pipeline so manual Daz work is minimal

---

## Toolchain

### 3D Software
- **Daz Studio** (free) — primary tool for characters, clothing, environments, rendering
  - Download: https://www.daz3d.com/get_studio
  - Use Genesis 9 figures for best realism

### MCP Server (bridges Claude → Daz Studio)
- **vangard-daz-mcp** by bluemoonfoundry
  - Repo: https://github.com/bluemoonfoundry/daz-mcp-server
  - Pure Python, no compiling required
  - ~22 high-level cinematic tools + full scene control
  - Requires the **DazScriptServer** plugin inside Daz Studio

### DazScriptServer Plugin (required by vangard-daz-mcp)
- Repo: https://github.com/bluemoonfoundry/daz-script-server
- Pre-built DLL — just copy into Daz Studio plugins folder
- Runs HTTP server on port 18811 inside Daz Studio
- Auto-generates API token at `~/.daz3d/dazscriptserver_token.txt`

---

## Setup Steps (Home PC — Windows)

### 1. Install Daz Studio
- Download and install from https://www.daz3d.com/get_studio
- Create a free Daz account if needed
- Install the **Genesis 9 Starter Essentials** content pack (free)

### 2. Install DazScriptServer Plugin
- Go to https://github.com/bluemoonfoundry/daz-script-server
- Download the pre-built DLL for your Daz Studio version (DS4 or DS6)
- Copy the DLL to: `C:\Program Files\DAZ 3D\DAZStudio4\plugins\`
- Restart Daz Studio — plugin starts automatically on port 18811

### 3. Install vangard-daz-mcp
```powershell
# Install uv package manager (if not already installed)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# Clone the MCP server
git clone https://github.com/bluemoonfoundry/daz-mcp-server.git C:\tools\daz-mcp-server
cd C:\tools\daz-mcp-server

# Install dependencies
uv sync
```

### 4. Configure Claude Desktop
Edit: `%APPDATA%\Claude\claude_desktop_config.json`

Add this block inside `"mcpServers"`:
```json
"vangard-daz-mcp": {
  "command": "uv",
  "args": [
    "run",
    "--project",
    "C:\\tools\\daz-mcp-server",
    "vangard-daz-mcp"
  ],
  "env": {
    "DAZ_HOST": "localhost",
    "DAZ_PORT": "18811"
  }
}
```

Restart Claude Desktop. Daz Studio tools will appear in Claude's tool palette.

### 5. Verify Connection
With Daz Studio open and DazScriptServer running, ask Claude:
> "List the tools available for Daz Studio"

If tools appear, the connection is working.

---

## Workflow Once Set Up

1. Open Daz Studio, load a base scene (environment + character)
2. Save character as a Scene Subset for reuse across all scenes
3. Tell Claude what you want: *"Put the character in the kitchen, standing by the counter, looking slightly left, neutral expression — render it"*
4. Claude controls Daz, triggers render, saves image
5. Repeat for each VN scene panel — same character, same consistency

---

## Asset Recommendations

### Free Starting Assets (Daz3D)
- Genesis 9 Starter Essentials (characters)
- Daz Studio default HDRI lighting
- Search Daz store during sales (70-90% off common)

### Free Community Assets
- https://www.renderosity.com (search "free")
- https://www.sharecg.com

### Paid but worth it
- Realistic interior environment sets: ~$15-30 each
- Character clothing bundles: ~$10-20 each (watch for sales)

---

## Alternative MCP (if needed)
- **DazPilot** by millsydotdev: https://github.com/millsydotdev/DazPilot
  - 196 commands, very comprehensive
  - Requires compiling from C++ source — more complex setup
  - Use vangard-daz-mcp first; fall back to this if needed

---

## Notes
- This project was started on a work PC (Windows 11) and is intended to continue on a home PC
- Claude Desktop is required (not Claude Code) for MCP tools to work with Daz Studio
- Keep Daz Studio open in the background while working — the MCP connects to the running instance
