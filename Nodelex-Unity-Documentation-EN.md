# Nodelex Unity Package — Documentation

**Version 1.0.0 · Unity 6+**  
**Dependency:** `com.unity.nuget.newtonsoft-json`

---

## Table of Contents

1. [Overview](#1-overview)
2. [Installation](#2-installation)
3. [Quick Start](#3-quick-start)
4. [Importing a Dialogue Tree](#4-importing-a-dialogue-tree)
5. [DialogueRunner](#5-dialoguerunner)
6. [Connecting Your UI](#6-connecting-your-ui)
7. [Handling Game Events](#7-handling-game-events)
8. [Multiple Dialogue Trees](#8-multiple-dialogue-trees)
9. [IDialogueUI Interface](#9-idialogueui-interface)
10. [Data Reference](#10-data-reference)
11. [Troubleshooting](#11-troubleshooting)

---

## 1. Overview

The Nodelex Unity Package bridges the **Nodelex** dialogue tree editor and your Unity game. Design dialogue trees visually in Nodelex, export them with a single button, and this package imports them as Unity ScriptableObject assets and runs them at runtime.

**What the package handles:**
- Importing `.unity.json` files from Nodelex into ScriptableObject assets
- Running dialogue trees at runtime via `DialogueRunner`
- Firing game events (quests, inventory, currency, etc.) defined in Nodelex
- Routing events to your game code via a clean C# API

**What you handle:**
- Building the dialogue UI (text, option buttons, speaker name)
- Responding to game events (awarding gold, starting quests, etc.)
- Calling `StartDialogue()` when the player interacts with an NPC

---

## 2. Installation

### Step 1 — Add the Scripts

Copy the `Nodelex/` folder into your project's `Assets/` directory:

```
Assets/
└── Nodelex/
    ├── Editor/
    └── Runtime/
```

### Step 2 — Install Newtonsoft.Json

1. Open **Window → Package Manager**
2. Click **+** → **Add package by name**
3. Enter: `com.unity.nuget.newtonsoft-json`
4. Click **Add**

### Step 3 — Verify

Open **Window → Nodelex → Import Dialogue Tree**. If the window opens without errors, installation is complete.

---

## 3. Quick Start

This is the fastest path from zero to a running dialogue. For a full working example with a 2D scene, NPC interaction, HUD, and game events, see the **Sample Project** included in the package.

**Step 1 — Export from Nodelex**

In the Nodelex app, click **Export to Unity** from the Editor menu. Two files are written to the `export/` folder:

```
export/
├── your-tree.unity.json
└── characters.json
```

**Step 2 — Import into Unity**

Open **Window → Nodelex → Import Dialogue Tree**, select your `.unity.json` file, and click **Import**. Assets are created at `Assets/DialogueTrees/`.

**Step 3 — Set up the NPC**

Add `DialogueRunner` to your NPC GameObject. Drag the created `DialogueTreeSO` onto the **Dialogue Tree** field.

**Step 4 — Start the dialogue**

```csharp
private DialogueRunner _runner;

private void Awake()
{
    _runner = GetComponent<DialogueRunner>();
}

public void Interact()
{
    _runner.StartDialogue();
}
```

**Step 5 — Listen for nodes and options**

```csharp
_runner.OnNodeEntered      += node    => Debug.Log(node.dialogueText);
_runner.OnOptionsPresented += options => Debug.Log($"{options.Count} options");
_runner.OnDialogueEnded    += ()      => Debug.Log("Dialogue ended");
```

That's the minimum setup. See the following sections for UI integration, event handling, and advanced usage.

---

## 4. Importing a Dialogue Tree

### 4.1 Export from Nodelex

Click **Export to Unity** in the Nodelex Editor menu. This produces:

- **`your-tree.unity.json`** — the dialogue tree
- **`characters.json`** — all characters in your Nodelex project

These two files must be in the same folder when importing.

### 4.2 Manual Import — Editor Window

1. Open **Window → Nodelex → Import Dialogue Tree**
2. Click **Browse…** and select your `.unity.json` file
3. The window shows `✓ characters.json detected` if found in the same folder
4. Set **Output Folder** (default: `Assets/DialogueTrees`)
5. Set **Characters Folder** — all characters are stored here, shared across all trees (default: `Assets/DialogueTrees/Characters`)
6. Click **Import**

### 4.3 Auto Import — Drag & Drop

Drag a `.unity.json` file directly into the Unity Project window. The package detects it automatically and runs the importer. `characters.json` is auto-detected if it is in the same folder.

### 4.4 Re-importing

Re-importing the same tree **updates** existing assets in place. Scene references and Inspector assignments are preserved — no need to re-assign your `DialogueTreeSO` after updating a tree in Nodelex.

### 4.5 Asset Structure After Import

```
Assets/DialogueTrees/
├── Characters/             ← shared across all trees
│   ├── Josh.asset
│   └── Guard.asset
├── TavernScene/
│   ├── TavernScene.asset   ← assign this to DialogueRunner
│   └── Nodes/
│       ├── node_abc.asset
│       └── node_def.asset
└── DungeonScene/
    ├── DungeonScene.asset
    └── Nodes/
```

---

## 5. DialogueRunner

`DialogueRunner` is the MonoBehaviour that drives a `DialogueTreeSO` at runtime. Add it to your NPC GameObject.

### 5.1 Inspector Fields

| Field | Type | Description |
|---|---|---|
| **Dialogue Tree** | `DialogueTreeSO` | The tree to run. Can also be set in code. |
| **Dialogue UI Component** | `MonoBehaviour` | Optional. A script implementing `IDialogueUI`. |
| **Auto Advance Statements** | `bool` | If enabled, Statement nodes advance automatically after a delay. |
| **Statement Auto Delay** | `float` | Seconds before a Statement node auto-advances. Default: `2`. |

### 5.2 Unity Events (Inspector wiring)

| Event | Fires when |
|---|---|
| `onDialogueStarted` | `StartDialogue()` is called |
| `onDialogueEnded` | End node is reached or `StopDialogue()` is called |
| `onSpeakerChanged` | The speaking character changes — passes the character name as `string` |

### 5.3 C# Events

```csharp
// Fires when a visible node becomes active (Starter, Dialogue, Statement)
runner.OnNodeEntered += (DialogueNodeSO node) => { };

// Fires when options are ready for the player to choose
// Only fires for Starter and Dialogue nodes — never for Statement or End
runner.OnOptionsPresented += (IReadOnlyList<DialogueOption> options) => { };

// Fires when a dialogue Event node is traversed
runner.OnDialogueEvent += (DialogueEventPayload payload) => { };

// Fires when the dialogue ends
runner.OnDialogueEnded += () => { };
```

### 5.4 Public API

```csharp
// Start using the tree assigned in the Inspector
runner.StartDialogue();

// Start with a specific tree
runner.StartDialogue(DialogueTreeSO tree);

// Select a player option — 0-based index
runner.SelectOption(int index);

// Advance past a Statement node
// Only valid when the current node has no options
runner.Advance();

// Immediately stop the dialogue
runner.StopDialogue();

// Read current state
bool           runner.IsRunning;
DialogueNodeSO runner.CurrentNode;
DialogueTreeSO runner.ActiveTree;
```

### 5.5 Node Behaviour at Runtime

| Node Type | What it does | How to advance |
|---|---|---|
| **Starter** | First node. Has dialogue text and options. | `SelectOption(index)` |
| **Dialogue** | A line with one or more player responses. | `SelectOption(index)` |
| **Statement** | A line with no player response. | `Advance()` or auto-advance |
| **Event** | Invisible to the player. Fires a game event and passes through automatically. | Automatic |
| **End** | Ends the dialogue. Fires `OnDialogueEnded`. | Automatic |

**Event node transparency:** When a player selects an option that passes through one or more Event nodes before reaching the next visible node, all events fire in sequence. The player sees no interruption — it is identical to a direct connection.

---

## 6. Connecting Your UI

The recommended approach is to subscribe to `DialogueRunner`'s C# events directly from your UI script. This keeps your UI independent of the Nodelex package.

### 6.1 Recommended Pattern

```csharp
public class DialoguePanel : MonoBehaviour
{
    [SerializeField] private TMP_Text      speakerNameText;
    [SerializeField] private TMP_Text      dialogueBodyText;
    [SerializeField] private Transform     optionsContainer;
    [SerializeField] private GameObject    optionButtonPrefab;
    [SerializeField] private Button        continueButton;

    private DialogueRunner _runner;

    public void Initialize(DialogueRunner runner)
    {
        _runner = runner;
        _runner.OnNodeEntered      += HandleNodeEntered;
        _runner.OnOptionsPresented += HandleOptions;
        _runner.OnDialogueEnded    += HandleEnd;

        continueButton.gameObject.SetActive(false);
        continueButton.onClick.AddListener(() => _runner.Advance());
    }

    private void OnDestroy()
    {
        if (_runner == null) return;
        _runner.OnNodeEntered      -= HandleNodeEntered;
        _runner.OnOptionsPresented -= HandleOptions;
        _runner.OnDialogueEnded    -= HandleEnd;
    }

    private void HandleNodeEntered(DialogueNodeSO node)
    {
        speakerNameText.text  = node.character != null ? node.character.characterName : string.Empty;
        dialogueBodyText.text = node.dialogueText;
        continueButton.gameObject.SetActive(node.IsStatementNode);

        foreach (Transform child in optionsContainer)
            Destroy(child.gameObject);
    }

    private void HandleOptions(IReadOnlyList<DialogueOption> options)
    {
        foreach (Transform child in optionsContainer)
            Destroy(child.gameObject);

        for (int i = 0; i < options.Count; i++)
        {
            int capturedIndex = i;
            var obj = Instantiate(optionButtonPrefab, optionsContainer);
            obj.GetComponentInChildren<TMP_Text>().text = options[i].text;
            obj.GetComponent<Button>().onClick.AddListener(() =>
                _runner.SelectOption(capturedIndex));
        }
    }

    private void HandleEnd()
    {
        // Show a continue button before closing, 
        // so the last node's text is visible before the panel disappears
        continueButton.gameObject.SetActive(true);
        continueButton.onClick.RemoveAllListeners();
        continueButton.onClick.AddListener(() => Destroy(gameObject));
    }
}
```

### 6.2 Important: Unsubscribe on Destroy

Always unsubscribe from runner events in `OnDestroy`. If the panel is destroyed while still subscribed, the runner will try to invoke methods on a destroyed object and throw a `MissingReferenceException` on the next dialogue.

### 6.3 Spawning the Panel

A common pattern is to instantiate the panel when dialogue starts and destroy it when it ends:

```csharp
// In your NPC or manager script:
private void OnInteract()
{
    var panel = Instantiate(dialoguePanelPrefab, hudTransform);
    panel.GetComponent<DialoguePanel>().Initialize(_runner);
    _runner.StartDialogue();
}
```

### 6.4 Conditional Advance

`Advance()` is public and can be called from anywhere. This means you can gate a Statement node behind any condition — a button press, an animation completing, a coroutine, or any game event:

```csharp
// Button press
continueButton.onClick.AddListener(() => _runner.Advance());

// After a coroutine
private IEnumerator PlayAnimationThenAdvance()
{
    yield return animator.PlayAnimation("entrance");
    _runner.Advance();
}

// After a timed delay
private IEnumerator DelayedAdvance(float seconds)
{
    yield return new WaitForSeconds(seconds);
    _runner.Advance();
}
```

---

## 7. Handling Game Events

Dialogue Event nodes fire game-side logic — awarding currency, starting quests, updating inventory, playing sounds, triggering camera movements.

### 7.1 DialogueEventListener (Recommended)

Subclass `DialogueEventListener` and add it to the **same GameObject** as `DialogueRunner`. Register a handler for each event in `Awake`.

```csharp
using Nodelex;

public class MarketEventHandler : DialogueEventListener
{
    protected override void Awake()
    {
        base.Awake(); // Required — must be the first line

        Register("evt_gold_acquired", OnGoldAcquired);
        Register("evt_quest_start",   OnQuestStarted);
        Register("evt_camera_zoom",   OnCameraZoom);
    }

    private void OnGoldAcquired(DialogueEventPayload payload)
    {
        float amount = payload.Get<float>("amount");
        GameManager.Instance.AddGold(amount);
    }

    private void OnQuestStarted(DialogueEventPayload payload)
    {
        QuestManager.Instance.StartQuest("main_quest");
    }

    private void OnCameraZoom(DialogueEventPayload payload)
    {
        StartCoroutine(CameraManager.ZoomIn());
    }
}
```

**Register by event name, not ID.** Event IDs in Nodelex are auto-generated UUIDs. Use the event name you defined in Nodelex instead (e.g. `"evt_gold_acquired"`).

#### Catching all events

Override `OnEventReceived` to run code for every event, in addition to individual handlers:

```csharp
protected override void OnEventReceived(DialogueEventPayload payload)
{
    Analytics.Track("dialogue_event", payload.EventName);
}
```

#### Catching unregistered events

Override `OnUnhandledEvent` to handle events with no registered handler:

```csharp
protected override void OnUnhandledEvent(DialogueEventPayload payload)
{
    Debug.LogWarning($"No handler for: {payload.EventName}");
}
```

### 7.2 Direct Subscription

For simpler cases, subscribe to `OnDialogueEvent` directly:

```csharp
_runner.OnDialogueEvent += payload =>
{
    if (payload.EventName == "evt_gold_acquired")
        GameManager.Instance.AddGold(payload.Get<float>("amount"));
};
```

### 7.3 DialogueEventPayload

```csharp
payload.EventId     // string — auto-generated UUID from Nodelex
payload.EventName   // string — the name you gave the event in Nodelex
payload.Parameters  // IReadOnlyDictionary<string, object>

// Type-safe parameter access by parameter name
float  amount  = payload.Get<float>("amount");
string source  = payload.Get<string>("source");
bool   enabled = payload.Get<bool>("isActive");
```

---

## 8. Multiple Dialogue Trees

An NPC can have multiple trees — one for the first meeting, another after a quest, another after a betrayal. Switch between them at runtime:

```csharp
public class Maren : NPC
{
    [SerializeField] private DialogueTreeSO firstMeetTree;
    [SerializeField] private DialogueTreeSO afterQuestTree;

    private bool _questStarted;

    public override void Interact(GameObject instigator)
    {
        var tree = _questStarted ? afterQuestTree : firstMeetTree;
        _runner.StartDialogue(tree);
    }

    // Call this from your event handler when the quest starts
    public void OnQuestStarted()
    {
        _questStarted = true;
    }
}
```

All trees for a character are imported and stored as separate `DialogueTreeSO` assets. Assign them in the Inspector and switch between them in code as shown above.

---

## 9. IDialogueUI Interface

`IDialogueUI` is an optional alternative to subscribing to C# events. Implement it on your UI MonoBehaviour and assign it to `DialogueRunner`'s **Dialogue UI Component** field. The runner will call its methods automatically.

```csharp
public interface IDialogueUI
{
    // Called for Starter and Dialogue nodes
    void ShowDialogue(string speakerName, string text, IReadOnlyList<DialogueOption> options);

    // Called for Statement nodes
    // Call runner.Advance() when the player is ready to continue
    void ShowStatement(string speakerName, string text);

    // Called when the dialogue ends
    void HideDialogue();
}
```

Use `IDialogueUI` when you want a clean interface contract. Use C# events (Section 6) when you need more control or want to keep your UI completely decoupled.

---

## 10. Data Reference

### DialogueTreeSO

| Field | Type | Description |
|---|---|---|
| `treeName` | `string` | Display name |
| `sourceFile` | `string` | Original export filename |
| `nodes` | `List<DialogueNodeSO>` | All nodes |
| `edges` | `List<DialogueEdge>` | All connections |
| `characters` | `List<CharacterSO>` | Characters referenced by this tree |

### DialogueNodeSO

| Field | Type | Description |
|---|---|---|
| `nodeId` | `string` | Unique identifier |
| `nodeType` | `NodeType` | Starter / Dialogue / Statement / Event / End |
| `character` | `CharacterSO` | Speaking character, or `null` |
| `dialogueText` | `string` | Line of dialogue |
| `options` | `List<DialogueOption>` | Player choices |
| `eventId` | `string` | UUID (Event nodes only) |
| `eventName` | `string` | Display name (Event nodes only) |
| `eventParameters` | `List<EventParameter>` | Parameters (Event nodes only) |
| `IsStatementNode` | `bool` | Helper property |
| `IsEventNode` | `bool` | Helper property |
| `IsTerminal` | `bool` | True if End node |
| `HasOptions` | `bool` | True if options list is non-empty |

### CharacterSO

| Field | Type | Description |
|---|---|---|
| `characterId` | `string` | Unique identifier matching Nodelex |
| `characterName` | `string` | Display name |
| `category` | `string` | e.g. `"NPC"`, `"Player"` |
| `avatar` | `Sprite` | Optional — assign manually in Unity |

### DialogueOption

| Field | Type | Description |
|---|---|---|
| `id` | `string` | Internal ID used by the edge system |
| `text` | `string` | Text shown to the player |

---

## 11. Troubleshooting

**MissingReferenceException on second dialogue**  
Your UI panel was destroyed without unsubscribing from runner events. Add `OnDestroy` to your UI script and unsubscribe all handlers. See Section 6.2.

**"No Starter node found"**  
The imported tree has no Starter node. Check in Nodelex that your tree has exactly one Starter node and re-export.

**Characters are empty after import**  
`characters.json` was not found during import. Make sure it is in the same folder as your `.unity.json` file.

**`base.Awake()` missing — events not firing**  
If you subclass `DialogueEventListener` and override `Awake`, `base.Awake()` must be the first line. Without it, the runner connection is never established and all `Register` calls have no effect.

**Option button index is always the same**  
You are capturing the loop variable directly. Capture it with a local variable inside the loop:
```csharp
int capturedIndex = i; // correct
btn.onClick.AddListener(() => _runner.SelectOption(capturedIndex));
```

**Event handler not called**  
You are registering by event ID (UUID). Register by event name instead — the name you gave the event in Nodelex:
```csharp
Register("evt_gold_acquired", OnGoldAcquired); // correct
```

**Dialogue ends without showing the last node**  
The End node fires `OnDialogueEnded` immediately. Show a continue button in `HandleEnd` and destroy the panel only when it is clicked. See Section 6.1.

**`payload.Get<T>` throws KeyNotFoundException**  
The parameter name does not match. Keys are the parameter names defined in Nodelex (e.g. `"amount"`), not numeric or UUID-based IDs.

**Dialogue starts but nothing appears on screen**  
Check that your UI script subscribed to runner events *before* `StartDialogue()` was called. If `Initialize` is called after `StartDialogue`, the first `OnNodeEntered` event will have already fired.
