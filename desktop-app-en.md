# Nodelex Desktop App — Documentation

**Version 0.1.0 · Windows / macOS / Linux**  
**Requires:** Python backend (bundled)

---

## Table of Contents

1. [Overview](#1-overview)
2. [Installation](#2-installation)
3. [Getting Started](#3-getting-started)
4. [Projects](#4-projects)
5. [Dialogue Trees](#5-dialogue-trees)
6. [Node Types](#6-node-types)
7. [Characters](#7-characters)
8. [Events](#8-events)
9. [Canvas Controls](#9-canvas-controls)
10. [Import & Export](#10-import--export)
11. [Unity Export](#11-unity-export)
12. [AI Features](#12-ai-features)
13. [Keyboard Shortcuts](#13-keyboard-shortcuts)

---

## 1. Overview

Nodelex Desktop is the full-featured NPC dialogue tree editor. It includes everything in the web editor plus AI-assisted dialogue generation, Unity engine export, and local data persistence.

**What you can do:**
- Create and manage projects with multiple dialogue trees and characters
- Build branching conversations visually
- Use AI to populate nodes or generate entire dialogue trees
- Export directly to Unity

**Your data is stored locally** in a SQLite database on your machine.

---

## 2. Installation

1. Download the latest release from **nodelex.online**
2. Run the installer
3. Launch **Nodelex** from your applications

The Python backend starts automatically when the app opens and shuts down when you close it.

---

## 3. Getting Started

1. Launch the app
2. Click **+ New Project** to create your first project
3. Enter a name and an optional universe context
4. Your first tree **"Main Dialogues"** is created automatically
5. Double-click the tree card to open the editor

---

## 4. Projects

### Creating a Project
Click **+ New Project**. Enter:
- **Name**
- **Universe Context** *(optional)* — describe your game world. Time period, factions, tone. This context is sent to the AI when generating dialogue.

### Editing a Project
Click **⋯** on a project card → **Edit**.

### Deleting a Project
Click **⋯** → **Delete**. Permanently removes the project and all its data.

### Unity Project Path
In the editor, go to **Editor menu → Export to Unity → Locate Unity Project** to link a Unity project folder. Once set, use **Commit** to export directly without a file dialog.

---

## 5. Dialogue Trees

### Creating a Tree
On the Project screen (Trees tab), click **+ New Tree**. Set a name and language.

### Tree Properties
Select a tree to open the Properties sidebar:
- **Name** — editable inline
- **Language** — dialogue will be generated in this language by AI
- **Tags** — for filtering

### Tags
Create and assign tags in the Properties sidebar. Filter trees by tag using the filter bar.

### Opening a Tree
Double-click a tree card or click **Open Editor** in the sidebar.

---

## 6. Node Types

| Node | Input | Output | Use |
|---|---|---|---|
| **Starter** | None | One per option | Opens the conversation |
| **Dialogue** | 1 | One per option | Continues with player choices |
| **Statement** | 1 | 1 | Character speaks, no player response |
| **End** | Many | None | Closes a branch |
| **Event** | 1 | 1 | Triggers a game event silently |

### Adding Nodes
- **Drag** from the left sidebar
- **Right-click** canvas → select node type

### Connecting Nodes
Drag from an output pin to an input pin.

**Edge rules:**
- Each output pin → exactly one target
- Starter, Dialogue, Statement, Event nodes → one incoming edge maximum
- End nodes → multiple incoming edges allowed

### Event Nodes
Invisible to the player at runtime. Fire a game event and pass through automatically. Transparent in exports — the connection bridges through as if the event node were not there.

---

## 7. Characters

Characters are stored per project and shared across all trees.

### Creating a Character
Characters tab → **+ New Character**:
- **Name**
- **Category** — NPC, Player, Merchant, Enemy, Ally, Quest Giver, Other
- **Color** — avatar color

### Character Properties
Click a character to open the sidebar:
- Edit name, category, color
- Add / remove **traits** — these are sent to the AI when generating dialogue for this character

### Using Characters in Nodes
Select from the **Character** dropdown in any node. Characters used in the current tree appear at the top. Click **+ Add Character** to create one without leaving the editor.

---

## 8. Events

Events represent game-side actions triggered during a dialogue — awarding currency, starting quests, updating inventory.

### Creating an Event
Events tab → **+ New Event**:
- **Name** — e.g. "Acquire Gold"
- **Parameters** — typed fields the event carries

### Parameter Types
| Type | Description |
|---|---|
| `text` | String — item name, quest ID |
| `number` | Numeric — gold amount |
| `boolean` | True / false |

### Using Events in Nodes
Add an **Event** node. Select an event from the dropdown — parameters appear as input fields.
Click **+ Add Event** to create a new event inline.

---

## 9. Canvas Controls

### Navigation
| Action | How |
|---|---|
| Pan | Middle mouse button drag |
| Pan (alternative) | Space + left click drag |
| Zoom | Scroll wheel |
| Fit view | Ctrl + Shift + F |

### Selection
| Action | How |
|---|---|
| Select node | Left click |
| Lasso select | Left click drag on canvas |
| Add to selection | Shift + click |
| Select all | Ctrl + A |
| Deselect | Escape |

### Editing
| Action | How |
|---|---|
| Delete selected | Delete or Backspace |
| Duplicate | Ctrl + D |
| Copy | Ctrl + C |
| Paste | Ctrl + V |
| Undo | Ctrl + Z |
| Redo | Ctrl + Y or Ctrl + Shift + Z |
| Save | Ctrl + S |

### Context Menu — Canvas
Right-click on empty canvas:
- Add Starter / Dialogue / Statement / End / Event node
- **✦ Create With AI** — generate a full dialogue tree

### Context Menu — Node
Right-click on a node:
- **✦ Populate With AI** — fill this node using AI
- Duplicate Node
- Delete Node

---

## 10. Import & Export

Access via the **Editor** menu in the title bar.

### Transfer (Import / Export)
Lossless format — preserves all node data, positions, and connections. Use for backup or moving trees between projects.

| Format | Description |
|---|---|
| JSON | Full tree data |
| CSV | Flat row-per-option format |

### Export Flow
Human-readable output showing the dialogue flow:

| Format | Description |
|---|---|
| Excel | Colour-coded rows, target previews |
| JSON | Structured flow data |
| CSV | Flat flow data |

---

## 11. Unity Export

Export your dialogue tree directly to a Unity project.

### Setup
1. Open the editor for a tree
2. **Editor menu → Export to Unity → Locate Unity Project**
3. Select your Unity project root folder (the folder containing `Assets/`)
4. Nodelex creates `Assets/Export/` automatically

### Committing
After setup, use **Export to Unity → Commit** to write the files instantly — no dialog.

Each commit writes two files:
- `{TreeName}.unity.json` — the dialogue tree
- `characters.json` — all characters in your project

### Manual Export
Use **Export to Unity → Export...** to choose a custom save location.

### In Unity
See the **[Unity Package Documentation](#)** for how to import and use these files.

---

## 12. AI Features

AI features require an API key from a supported provider.

### Setting Up
1. **Settings menu → AI Settings**
2. Select a provider (Gemini, Claude, or OpenAI)
3. Select a model
4. Enter your API key and click **Save Key**

### Populate With AI
Right-click a node → **✦ Populate With AI**

Fills the node's dialogue text and options based on:
- The character's name, category, and traits
- The upstream conversation context (up to 10 previous nodes)
- Your instruction

The character must be assigned to the node before using this feature.

### Create With AI
Right-click on the canvas → **✦ Create With AI**

Generates a complete dialogue tree from scratch based on:
- Selected characters and their traits
- Your scene context description
- The tree's language setting
- The project's universe context

The generated nodes are added to the canvas. Review and save when ready.

### Supported Providers

| Provider | Free Tier | Notes |
|---|---|---|
| **Gemini Flash** | ✓ Yes | Recommended for most users |
| **Claude** | Limited | Higher quality output |
| **OpenAI** | No | GPT-4o and variants |

---

## 13. Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| Ctrl + S | Save tree |
| Ctrl + Z | Undo |
| Ctrl + Y | Redo |
| Ctrl + Shift + Z | Redo (alternative) |
| Ctrl + A | Select all |
| Ctrl + D | Duplicate selected |
| Ctrl + C | Copy selected |
| Ctrl + V | Paste at mouse position |
| Ctrl + Shift + F | Fit view |
| Delete / Backspace | Delete selected |
| Escape | Deselect / close menu |
| Space + drag | Pan canvas |
