# 📝 extract-commit-message

**Parse the latest commit message into head and body for automated PR workflows.**  
This composite GitHub Action extracts structured outputs from the most recent commit, enabling clean PR titles, bodies, and conditional logic.

---

## 🚀 Overview

`extract-commit-message` reads the latest commit message and splits it into:

- `head_message`: First line (used as PR title)
- `body_message`: Remaining lines (used as PR body)
- `is_multiline`: Boolean flag for conditional logic

If the commit message is a single line, it provides a fallback body for automation clarity.

---

## 📦 Usage

### Minimal Example

```yaml
jobs:
  parse-commit:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Extract commit message
        id: commit
        uses: owen-6936/extract-commit-message@v1

      - name: Log results
        run: |
          echo "Head: ${{ steps.commit.outputs.head_message }}"
          echo "Body: ${{ steps.commit.outputs.body_message }}"
          echo "Multiline: ${{ steps.commit.outputs.is_multiline }}"
```

### With PR Creation

```yaml
jobs:
  create-pr:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Extract commit message
        id: commit
        uses: owen-6936/extract-commit-message@v1

      - name: Create pull request
        uses: owen-6936/create-pr@v1
        with:
          title: ${{ steps.commit.outputs.head_message }}
          body: ${{ steps.commit.outputs.body_message }}
```

---

## ⚙️ Outputs

| Name           | Description                                           |
|----------------|-------------------------------------------------------|
| `head_message` | First line of the commit message                      |
| `body_message` | Remaining lines of the commit message (or fallback)   |
| `is_multiline` | `true` if the message spans multiple lines            |

---

## 🧠 Behavior

- If the commit message has multiple lines:
  - First line becomes `head_message`
  - Remaining lines become `body_message`
  - `is_multiline` is `true`
- If the commit message is a single line:
  - That line becomes `head_message`
  - `body_message` defaults to:  
    `"This pull request was automatically created by a workflow."`
  - `is_multiline` is `false`

---

## 🔍 Example

Given this commit:

```bash
feat: add user onboarding flow

Includes QR preview, SVG accents, and reviewer guide scaffolding.
```

The outputs will be:

```yaml
head_message: feat: add user onboarding flow
body_message: Includes QR preview, SVG accents, and reviewer guide scaffolding.
is_multiline: true
```

---

## 🧩 Philosophy

- **Modular**: No assumptions about commit format or structure
- **Reviewer-aware**: Provides fallback body to ensure clarity
- **Composable**: Designed to pair with PR creation, changelog generation, and release workflows
- **Expressive**: Outputs are structured, readable, and easy to consume

---

## 🔮 Roadmap

- `v1.1.0`: Support for parsing multiple commits in a push
- `v1.2.0`: Optional truncation or sanitization of body
- `v1.3.0`: Markdown formatting for changelog integration

---

## 🛡️ License

MIT — build freely, automate kindly.

---

## 💬 Feedback

Open an issue or discussion if you hit an edge case or want to extend support.  
This action is part of the **expo-ci basic family** — modular DX tools for expressive CI.
