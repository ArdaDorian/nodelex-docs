# Nodelex Web Editor — Documentation

**Version 1.0.0 · editor.nodelex.online**  
**No installation required — runs in your browser**

---

## Table of Contents

1. [Overview](#1-overview)
2. [Getting Started](#2-getting-started)
3. [Projects](#3-projects)
4. [Dialogue Trees](#4-dialogue-trees)
5. [Node Types](#5-node-types)
6. [Characters](#6-characters)
7. [Events](#7-events)
8. [Canvas Controls](#8-canvas-controls)
9. [Import & Export](#9-import--export)
10. [Keyboard Shortcuts](#10-keyboard-shortcuts)

---

## 1. Overview

The Nodelex Web Editor is a free, browser-based dialogue tree editor. No signup, no installation — open the page and start building.

**What you can do:**
- Create and manage projects with multiple dialogue trees
- Build branching conversations using a visual node-based canvas
- Organize characters and game events in a shared library
- Export your work as JSON, CSV, or Excel

**What is not available in the web editor:**
- AI-assisted dialogue generation (desktop app only)
- Unity / Unreal Engine direct export (desktop app only)

**Your data is stored locally in your browser** using IndexedDB. Clearing browser data will delete your projects.

---

## 2. Getting Started

1. Open **editor.nodelex.online**
2. Click **+ New Project** to create your first project
3. Enter a project name and an optional universe context
4. You will be taken to the Project screen — your first tree **"Main Dialogues"** is created automatically
5. Double-click the tree card to open the editor

---

## 3. Projects

### Creating a Project
Click the **+ New Project** card on the Project Selection screen. Enter:
- **Name** — your project name
- **Universe Context** *(optional)* — a short description of your game world. Helps keep dialogue consistent across trees.

### Editing a Project
Click the **⋯** menu on a project card → **Edit**. You can update the name and universe context.

### Deleting a Project
Click the **⋯** menu → **Delete**. This permanently removes the project and all its trees, characters, and events.

---

## 4. Dialogue Trees

Dialogue trees live inside a project. Each tree is an independent branching conversation.

### Creating a Tree
On the Project screen (Trees tab), click **+ New Tree**. Enter a name and select a language — all dialogue in this tree will be written in that language.

### Editing Tree Properties
Select a tree to open the Properties sidebar on the right:
- **Name** — edit inline, saves on blur or Enter
- **Language** — change the target language for this tree
- **Tags** — assign tags to organize and filter trees

### Tags
Create tags in the Properties sidebar. Assign them to trees and use the filter bar at the top to show only trees with a specific tag.

### Opening a Tree
Double-click a tree card, or select it and click **Open Editor** in the sidebar.

---

## 5. Node Types

| Node | Input | Output | Use |
|---|---|---|---|
| **Starter** | None | One per option | Opens the conversation |
| **Dialogue** | 1 | One per option | Continues with player choices |
| **Statement** | 1 | 1 | Character speaks, no player response |
| **End** | Many | None | Closes a branch |
| **Event** | 1 | 1 | Triggers a game event silently |

### Adding Nodes
- **Drag** a node type from the left sidebar onto the canvas
- **Right-click** on the canvas → select a node type to spawn it at that position

### Editing Nodes
Each node has:
- **Character** dropdown — assign a speaking character (or add a new one inline)
- **Dialogue / Line** text area — auto-resizes as you type
- **Options** *(Starter & Dialogue only)* — add player response choices, each gets its own output pin

### Connecting Nodes
Drag from an output pin to the input pin of another node.

**Rules:**
- Each output pin connects to exactly one target
- Starter, Dialogue, and Statement nodes accept only one incoming connection
- End nodes accept multiple incoming connections

### Event Nodes
Event nodes are invisible to the player at runtime. They fire a game event and pass through automatically. Select an event from the dropdown — parameters appear dynamically based on the event definition.

---

## 6. Characters

Characters are stored per project and shared across all trees.

### Creating a Character
Go to the **Characters** tab and click **+ New Character**. Set:
- **Name**
- **Category** — NPC, Player, Merchant, Enemy, etc.
- **Color** — used for the avatar circle

### Editing a Character
Click a character card to open the Properties sidebar:
- Edit name, category, color
- Add or remove **traits** (e.g. friendly, aggressive, mysterious)

### Using Characters in Nodes
In any node, click the **Character** dropdown. Characters already used in the current tree appear at the top for quick access.

You can also add a new character directly from the dropdown — click **+ Add Character** to create one without leaving the editor.

---

## 7. Events

Events represent game-side actions triggered during dialogue — awarding gold, starting quests, updating inventory.

### Creating an Event
Go to the **Events** tab and click **+ New Event**. Set:
- **Name** — e.g. "Acquire Gold", "Start Quest"
- **Parameters** — define the data the event carries

### Parameter Types
| Type | Use |
|---|---|
| `text` | String values — item names, quest IDs |
| `number` | Numeric values — gold amount, quantity |
| `boolean` | True/false flags |

### Using Events in Nodes
Add an **Event** node to the canvas. Select an event from the dropdown — its parameters will appear as input fields inside the node.

You can also add a new event directly from the dropdown — click **+ Add Event**.

---

## 8. Canvas Controls

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
| Lasso select | Left click drag on empty canvas |
| Add to selection | Shift + click |
| Select all | Ctrl + A |
| Deselect | Escape |

### Editing
| Action | How |
|---|---|
| Delete selected | Delete or Backspace |
| Duplicate | Ctrl + D (spawns at mouse position) |
| Copy | Ctrl + C |
| Paste | Ctrl + V (spawns at mouse position) |
| Undo | Ctrl + Z |
| Redo | Ctrl + Y or Ctrl + Shift + Z |
| Save | Ctrl + S |

### Context Menu
**Right-click on empty canvas:**
- Add Starter / Dialogue / Statement / End / Event node at cursor position

**Right-click on a node:**
- Duplicate Node
- Delete Node

---

## 9. Import & Export

Access via the **Import** and **Export** menus in the editor toolbar.

### Import
| Format | Use |
|---|---|
| **JSON** | Restore a previously exported dialogue tree |
| **CSV** | Import from spreadsheet format |

Importing replaces the current canvas. If you have unsaved changes, you will be asked to confirm.

### Export
| Format | Use |
|---|---|
| **JSON** | Lossless — all node data, positions, connections. Use for backup or transfer between projects. |
| **CSV** | Flat format — one row per option/branch. |

### Export Flow
Human-readable format showing the full dialogue flow:

| Format | Use |
|---|---|
| **Excel** | Colour-coded rows by node type, with target previews |
| **JSON** | Flow data as structured JSON |
| **CSV** | Flow data as a flat spreadsheet |

---

## 10. Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| Ctrl + S | Save tree |
| Ctrl + Z | Undo |
| Ctrl + Y | Redo |
| Ctrl + Shift + Z | Redo (alternative) |
| Ctrl + A | Select all |
| Ctrl + D | Duplicate selected nodes |
| Ctrl + C | Copy selected nodes |
| Ctrl + V | Paste nodes at mouse position |
| Ctrl + Shift + F | Fit view |
| Delete / Backspace | Delete selected nodes or edges |
| Escape | Deselect all / close context menu |
| Space + drag | Pan canvas |
