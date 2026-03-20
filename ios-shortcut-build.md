# iOS Shortcut: Acta Cogitationis Logger

## Prerequisites

1. Create a GitHub Personal Access Token (fine-grained):
   - Go to: github.com/settings/tokens?type=beta
   - Token name: `acta-cogitationis-logger`
   - Expiration: 1 year (or custom)
   - Repository access: Only select repositories → `acta-cogitationis`
   - Permissions: Contents → Read and write
   - Generate → copy the token

## Build the Shortcut

Open the Shortcuts app. Create a new shortcut. Name it **Log Cognition**.

### Step 1: Choose from Menu

- Action: **Choose from Menu**
- Prompt: `Category`
- Options (add 5):
  - `retrieval`
  - `working-memory`
  - `pattern`
  - `orientation`
  - `positive`

Each menu option section does the same thing — sets a variable:

- Inside each menu option, add: **Set Variable** → name: `category` → value: the category name (e.g., `retrieval`)

### Step 2: Ask for Input

- Action: **Ask for Input**
- Prompt: `Context (energy, sleep, stress)`
- Input type: Text
- Add: **Set Variable** → name: `context` → value: Provided Input

### Step 3: Ask for Input

- Action: **Ask for Input**
- Prompt: `What happened?`
- Input type: Text
- Add: **Set Variable** → name: `description` → value: Provided Input

### Step 4: Ask for Input

- Action: **Ask for Input**
- Prompt: `Resolution? (leave blank if unresolved)`
- Input type: Text
- Add: **Set Variable** → name: `resolution` → value: Provided Input

### Step 5: Format Date

- Action: **Date**
- Type: Current Date
- Action: **Format Date**
- Date Format: Custom → `yyyy-MM-dd`
- Set Variable → name: `entryDate`

- Action: **Date** (current date again)
- Action: **Format Date**
- Date Format: Custom → `HH:mm`
- Set Variable → name: `entryTime`

### Step 6: Get current file from GitHub

- Action: **Get Contents of URL**
- URL: `https://api.github.com/repos/DorcusTitanus/acta-cogitationis/contents/entries/2026.md`
- Method: GET
- Headers:
  - `Authorization`: `Bearer YOUR_TOKEN_HERE`
  - `Accept`: `application/vnd.github+json`
- Set Variable → name: `fileResponse`

### Step 7: Extract current content and SHA

- Action: **Get Dictionary Value**
- Key: `sha` from `fileResponse`
- Set Variable → name: `fileSha`

- Action: **Get Dictionary Value**
- Key: `content` from `fileResponse`
- Set Variable → name: `existingContentB64`

- Action: **Decode Base64**
- Input: `existingContentB64`
- Set Variable → name: `existingContent`

### Step 8: Build the new entry

- Action: **Text**
- Content:
```
## entryDate | entryTime | category

**Context:** context

description

**Resolution:** resolution
```

(Use the variable tokens from Shortcuts, not the literal words above.)

- Set Variable → name: `newEntry`

### Step 9: Combine existing + new

- Action: **Text**
- Content: `existingContent` + two newlines + `newEntry`
- Set Variable → name: `fullContent`

- Action: **Encode Base64**
- Input: `fullContent`
- Set Variable → name: `fullContentB64`

### Step 10: Push to GitHub

- Action: **Get Contents of URL**
- URL: `https://api.github.com/repos/DorcusTitanus/acta-cogitationis/contents/entries/2026.md`
- Method: PUT
- Headers:
  - `Authorization`: `Bearer YOUR_TOKEN_HERE`
  - `Accept`: `application/vnd.github+json`
- Request Body: JSON
  - `message`: `entry: entryDate category`
  - `content`: `fullContentB64`
  - `sha`: `fileSha`

### Step 11: Notification

- Action: **Show Notification**
- Title: `Acta Cogitationis`
- Body: `Entry logged: category`

## Usage

Say "Hey Siri, Log Cognition" or tap the shortcut. Four prompts, then it commits.

## Year Rollover

On January 1, create `entries/2027.md` in the repo with `# 2027` as the content, and update the URL in steps 6 and 10.

## Token Security

The token is stored inside the shortcut itself. If you share the shortcut, strip the token first. The token only has access to this one repo.
