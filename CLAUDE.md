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

## Asset Shopping List

Install all of these before the first Claude session. Use Daz Install Manager for Daz store items. Sales are frequent — check before buying, discounts of 70-90% are common.

### FREE — Install First

| Asset | Where | Notes |
|-------|-------|-------|
| [Genesis 9 Starter Essentials](https://www.daz3d.com/genesis-9-starter-essentials) | Daz Store | Base character, included with Daz Studio install |
| [Genesis 9 Starter Essentials Expansion](https://www.daz3d.com/genesis-9-starter-essentials-expansion) | Daz Store | Extra morphs and shapes, free |
| Modern Interiors HDRI | Daz Store | Realistic interior lighting, search "Modern Interiors HDRI" |

---

### Environments

| Asset | Where | Notes |
|-------|-------|-------|
| [i13 Executive Office Environment](https://www.daz3d.com/i13-executive-office-environment) | Daz Store | Best match for office/boss scene — 10 camera presets, reception, boardroom |
| [Executive Office and Conference](https://www.daz3d.com/executive-office-and-conference) | Daz Store | Alternative office — modern professional style |
| [FH Warehouse Room](https://www.daz3d.com/fh-warehouse-room) | Daz Store | Clean warehouse with metal racks and boxes — matches VN reference screenshots |
| [Industrial Warehouse Environment](https://www.daz3d.com/industrial-warehouse-environment) | Daz Store | Alternative warehouse — more industrial/gritty |

---

### Poses

| Asset | Where | Notes |
|-------|-------|-------|
| [Standing Conversation Poses 3 for Genesis 8, 8.1 and 9](https://www.daz3d.com/standing-conversation-poses-3-for-genesis-8-81-and-9) | Daz Store | Works on Genesis 9, male and female, various moods |
| [Z Conversation and Disagreement Pose Mega Set](https://www.daz3d.com/z-conversation-and-disagreement-pose-mega-set-for-genesis-8-and-81) | Daz Store | 30 poses + mirrors + 10 couple poses — great value |
| [Essential Office Poses for Genesis 8 Female](https://www.daz3d.com/essential-office-poses-for-genesis-8-female) | Daz Store | 25 office-specific poses, sitting/standing at desk |
| [Z Modern Office Chair and Poses](https://www.daz3d.com/z-modern-office-chair-and-poses-for-genesis-8-and-81) | Daz Store | 35 poses designed for sitting in office chairs — perfect for desk scenes |

---

### Female Clothing

| Asset | Where | Notes |
|-------|-------|-------|
| [dForce Women's Office Outfit for Genesis 9](https://www.daz3d.com/dforce-womens-office-outfit-for-genesis-9) | Daz Store | Jacket, shirt, skirt, shoes, glasses — multiple colours |
| [MS Business Woman Outfit for Genesis 9 and 8 Female](https://www.daz3d.com/ms-business-woman-outfit-for-genesis-9-and-8-female) | Daz Store | Pencil skirt + shirt + vest, 12 texture sets |
| [dForce Elegant Office Outfit for Genesis 9](https://www.daz3d.com/dforce-elegant-office-outfit-for-genesis-9) | Daz Store | Shirt, skirt, pumps — 6 colour options |

---

### Female Hair

| Asset | Where | Notes |
|-------|-------|-------|
| [dForce CS Hannah Hair for Genesis 9](https://www.daz3d.com/dforce-cs-hannah-hair-for-genesis-9) | Daz Store | Mid-length, realistic strands, simulates with poses |
| [dForce Strand-Based Beauty Style Long Hair for Genesis 9](https://www.daz3d.com/dforce-strand-based-beauty-style-long-hair-for-genesis-9-and-genesis-8-female) | Daz Store | Long flowing hair, matches VN reference style |
| [dForce CS Belle Hair for Genesis 9 and 8 Female](https://www.daz3d.com/dforce-cs-belle-hair-for-genesis-9-and-8-female) | Daz Store | Low bun with soft waves — professional look |

---

### Notes on Installing
- All Daz store links above go to the exact product page — add to cart and download via Daz Install Manager
- Genesis 8 pose packs work on Genesis 9 figures (Daz auto-converts them)
- Install everything before starting a Claude session — Claude can then browse and use any installed asset via MCP
- Free community assets also available at [Renderosity](https://www.renderosity.com) and [ShareCG](https://www.sharecg.com) — search "free" and filter by category

---

## Alternative MCP (if needed)
- **DazPilot** by millsydotdev: https://github.com/millsydotdev/DazPilot
  - 196 commands, very comprehensive
  - Requires compiling from C++ source — more complex setup
  - Use vangard-daz-mcp first; fall back to this if needed

---

## Clothing & Posing

### Clothing Swaps
Daz3D uses a **Smart Content / Fit To** system — swapping outfits is just double-clicking a clothing item from the library. It auto-conforms to the character's body shape instantly. Multiple items layer (shirt + jacket + pants + shoes). dForce simulates fabric drape on each new pose automatically. The character underneath stays identical across all scenes, so consistency is maintained.

### Posing via MCP
Claude can:
- Apply any pre-made pose from the installed library by name
- Blend and combine poses
- Adjust specific joints (head tilt, arm raise, leg cross, etc.)
- Move characters to exact positions within the scene
- Set facial expressions and eye direction
- Coordinate two characters (looking at each other, one reaching toward the other)

Practical workflow: Claude applies the closest pre-made pose, then fine-tunes specific joints. For a VN, pre-made pose libraries cover ~90% of needs (sitting, standing, talking, reacting).

### Scene Style Reference
The target visual style is similar to mainstream Ren'Py games — Daz3D + Iray rendering, purchased environment sets, Genesis 8/9 figures with skin textures and hair assets. See the reference images in this repo for the quality benchmark.

### Adult Content Split
This VN contains adult themes with consenting adult characters. The workflow split is:
- **Claude handles:** narrative scenes, romantic tension, suggestive framing, camera/lighting setups that imply rather than show
- **User handles directly in Daz Studio:** any explicitly sexual poses or scenes — the same tools, assets, and characters are available, just operated manually

---

## Downloading Assets — What Claude Can and Can't Do

Claude **cannot** automatically download or install assets from the Daz store or community sites. This requires logging into a Daz account and using their Daz Install Manager app.

**What needs to be done manually (by the user):**
- Log into daz3d.com, add free/paid items to cart, download via Daz Install Manager
- Download free zips from Renderosity or ShareCG and install manually into Daz Studio

**What Claude can do once assets are installed:**
- Browse the local Daz content library through the MCP
- Find, load, and apply any installed environment, character, clothing, or pose
- Everything from asset selection onward is fully automated

**Before the first session, manually install at minimum:**
- Genesis 9 Starter Essentials (included with Daz Studio install)
- At least one free interior environment from the Daz store (search "free" + filter by environment/architecture)
- A clothing outfit for your main character

---

## Notes
- This project was started on a work PC (Windows 11) and is intended to continue on a home PC
- Claude Desktop is required (not Claude Code) for MCP tools to work with Daz Studio
- Keep Daz Studio open in the background while working — the MCP connects to the running instance
