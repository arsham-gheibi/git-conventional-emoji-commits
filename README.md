# Git Conventional Emoji Commits 🚦✨

**Automagically prepend relevant emojis to your commit messages, based on [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)!**  
Make your git history more readable and fun, with zero manual effort.

---

## 🌟 Features

- **🐍 Language:** Python (portable and easy to edit)
- **⚙️ Supported commit types:** feat, fix, build, chore, docs, style, refactor, test, perf, ci, revert (customizable)
- **💻 Works everywhere:** Add to any repo, all repos at once, or make it your git default
- **📦 No dependencies:** Pure Python 3
- **🐇 Easy to extend:** Just edit the mapping in the hook

---

## 📦 How It Works

This repo provides a drop-in `commit-msg` hook.  
It checks your commit message for Conventional Commit prefixes (like `feat:`, `fix:`, etc.), and automatically prepends a matching emoji.

**Example:**

```
fix: resolve login bug
```

becomes

```
🐛 fix: resolve login bug
```

---

## 🚀 Quickstart

### 1. Clone this repo

```sh
git clone https://github.com/arsham-gheibi/git-conventional-emoji-commits.git
cd git-conventional-emoji-commits
```

### 2. Install in a single repository

Copy the `commit-msg` hook into your target repo:

```sh
cp commit-msg /path/to/your/repo/.git/hooks/commit-msg
chmod +x /path/to/your/repo/.git/hooks/commit-msg
```

### 3. Install in all your existing repositories

**If all repos are in `~/code`:**

```sh
find ~/code -type d -name ".git" | while read gitdir; do
    cp commit-msg "$gitdir/hooks/commit-msg"
    chmod +x "$gitdir/hooks/commit-msg"
    echo "Added hook to $gitdir"
done
```

_Change `~/code` to the parent directory of your git repos._

### 4. Make it default for all NEW repos

```sh
mkdir -p ~/.git-templates/hooks
cp commit-msg ~/.git-templates/hooks/commit-msg
chmod +x ~/.git-templates/hooks/commit-msg
git config --global init.templateDir ~/.git-templates
```

From now on, every `git init` will copy your hook automatically.

---

## 📝 Conventional Commit Types & Emojis

| Type     | Emoji | Example                         |
| -------- | ----- | ------------------------------- |
| feat     | ✨    | feat: add new login endpoint    |
| fix      | 🐛    | fix: correct user email parsing |
| build    | 📦    | build(deps): update poetry.lock |
| chore    | 🔧    | chore: update linter rules      |
| docs     | 📝    | docs: improve README            |
| style    | 💄    | style: reformat codebase        |
| refactor | ♻️    | refactor: simplify user logic   |
| test     | ✅    | test: add edge case tests       |
| perf     | ⚡️   | perf: speed up query            |
| ci       | 👷    | ci: update github actions       |
| revert   | ⏪    | revert: undo previous migration |

**Custom types?**  
Edit the `emoji_map` in the script to add more types or change emojis!

---

## 🐍 The Hook Script

Below is the full content for `commit-msg` (Python 3):

```python
#!/usr/bin/env python3

import sys
import re

# File containing the commit message
msg_file = sys.argv[1]

# Read the current commit message
with open(msg_file, 'r', encoding='utf-8') as f:
    msg = f.read()

# Map Conventional Commit types to emojis
emoji_map = {
    r'^feat:': '✨',
    r'^fix:': '🐛',
    r'^build:': '📦',
    r'^chore:': '🔧',
    r'^docs:': '📝',
    r'^style:': '💄',
    r'^refactor:': '♻️',
    r'^test:': '✅',
    r'^perf:': '⚡️',
    r'^ci:': '👷',
    r'^revert:': '⏪',
}

def get_emoji(msg):
    for pattern, emoji in emoji_map.items():
        if re.match(pattern, msg, re.IGNORECASE):
            return emoji
    return ''

lines = msg.splitlines()
if lines:
    emoji = get_emoji(lines[0])
    if emoji and not lines[0].startswith(emoji):
        lines[0] = f'{emoji} {lines[0]}'
        # Write back the modified message
        with open(msg_file, 'w', encoding='utf-8') as f:
            f.write('\n'.join(lines))

# No emoji added if commit type doesn't match
```

**Requirements:** Python 3 (no external packages).

---

## 💡 Pro Tips

- Write commit messages using [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- The hook will only add an emoji if the type matches.
- Already have an emoji? It won’t add a duplicate!
- Want to change mapping or logic? Edit the script—it’s <50 lines, pure Python!

---

## 📤 Contributing

PRs welcome!

- Add more commit types or emoji styles
- Port to other languages (bash, node, etc)
- Add installer or cross-platform support
- Share your feedback!

---

## 📄 License

MIT License

---

## ❤️ Credits

Inspired by [cz-emoji](https://github.com/ngryman/cz-emoji), Conventional Commits, and the open source community.

---

> Happy committing! Now your git log will make you smile 😁

---

**This file includes everything—usage, emoji table, full script, install in existing/new repos, tips, credits.**
If you want me to add badges, a project logo, or usage screenshots, just say the word!
