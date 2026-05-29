# AGENTS.md

## Repo type

This is an **Obsidian vault**. When editing notes (`.md`), use the `obsidian-markdown` skill. For `.canvas` files use `json-canvas`, for `.base` files use `obsidian-bases`. For vault search, task management, or plugin debugging use `obsidian-cli`. For extracting web content use `defuddle`.

## Vault structure

```
Note/
├── Linux/              # Linux notes (KVM, QEMU, Libvirt, Yazi, Rsync, etc.)
│   └── assets/         # Attachments per note
├── Rust/               # Rust notes (cargo, etc.)
├── 计算机网络/          # Computer network notes
├── Python/             # Python notes
├── JavaScript/         # JavaScript notes
├── 任务清单/            # Daily task lists
├── .obsidian/          # Obsidian config
└── .claude/            # Claude Code config
```

## Version control: Jujutsu, not Git

This repo uses **jj** (Jujutsu), **not** git. The remote is `origin git@github.com:shiwu84/Note.git`.

Every change session follows this exact order:

```
jj git fetch                  # pull latest from remote
# ... make changes ...
jj st                         # review changes
jj commit -m "描述"            # commit (Chinese message)
jj bookmark move main --to <提交ID>  # move bookmark (mandatory!)
jj git push                   # push to remote
```

> [!important] Bookmark move is mandatory
> After `jj commit`, the `main` bookmark does **not** auto-advance. You must run `jj bookmark move main --to <id>` before pushing, or nothing gets pushed.

### Quick reference

| Operation | Command |
|-----------|---------|
| Fetch remote | `jj git fetch` |
| Show status | `jj st` |
| Show history | `jj log --limit 5` |
| Commit | `jj commit -m "描述"` |
| Rebase onto remote | `jj rebase -s <commit> -d <remote-commit>` |
| Move bookmark | `jj bookmark move main --to <commit-id>` |
| Push | `jj git push` |
| Revert file | `jj restore <path>` |

> [!warning]
> - Do NOT write "Co-Authored-By: Claude" — that's a git convention, not needed for jj.
> - Commit messages in Chinese, concise description of changes.
> - `.obsidian/workspace.json` is in `.gitignore` — never commit it.

## Files never to commit

These are in `.gitignore` and must be left alone:

- `.obsidian/workspace.json` — personal workspace state, changes constantly
- `.obsidian/workspace-mobile.json` — mobile workspace state
- `.obsidian/plugins/*/main.js`, `manifest.json`, `styles.css` — plugin binaries, re-downloadable

## Editing principles

1. **Don't change the original meaning** — only make the changes the user requested. Do not rewrite, polish, or expand content on your own.
2. **Don't introduce ambiguity** — new or modified content must be clear and precise. Technical terms must remain accurate.
3. **Keep style consistent** — write in Chinese; follow all formatting conventions below.

## Optimization rules

When optimizing notes, follow these rules strictly:

### 1. Scan before editing

Before modifying any note, scan the entire vault for optimization opportunities:
- Plain text that can be converted to tables (comparisons, parameter lists)
- Long paragraphs that can be split into lists
- Key terms/parameters not yet bolded or highlighted
- Steps missing callout hints
- Existing notes that can be linked with bidirectional wikilinks

### 2. Use markdown syntax fully

No long plain-text paragraphs allowed. Choose the best format based on content semantics:

| Content type | Use |
|-------------|-----|
| Step-by-step procedures | Ordered lists |
| Parallel items | Unordered lists |
| Comparisons, parameter tables | Tables |
| Commands/code/paths/package names | Code blocks or inline code |
| Key terms, first-appearance concepts | **Bold** |
| Key parameters, config values, error-prone points | ==Highlight== |
| Warnings, tips, notes | Callouts |

### 3. Use bidirectional links fully

- Every note must link to all related notes via `[[wikilinks]]`
- Links must be bidirectional: if A links to B, B must link back to A
- Task list items must link to corresponding knowledge notes
- After creating a new note, scan existing notes for mentions of the topic and add backlinks

### 4. After editing checklist

- [ ] Complete frontmatter (date, tags)
- [ ] Exactly one H1 title
- [ ] Clear heading hierarchy, no skipped levels
- [ ] Long plain text converted to lists or tables
- [ ] Key terms/parameters bolded or highlighted
- [ ] All commands/paths/package names in code blocks or inline code
- [ ] Appropriate callouts used (at least one per note)
- [ ] Bidirectional wikilinks established with related notes
- [ ] Task list items linked to knowledge notes
- [ ] Vault scanned for missing backlinks

## Obsidian config facts

- **Wikilinks use absolute paths**: `[[Linux/KVM]]`, `[[Linux/KVM#标题]]`, `[[Linux/KVM|别名]]`
- **`alwaysUpdateLinks: true`** — if you rename or move a note, Obsidian auto-updates incoming links
- **Attachment storage**: custom plugin stores attachments in `./assets/<NoteName>/`
- **Frontmatter required** on every note: `date: YYYY-MM-DD` and `tags` (at least 2, hierarchical: `tag/subtag`)
- **Task notes**: `tags` must include `tasks/daily` (daily tasks) or `tasks` (other tasks)
- **Notes are written in Chinese**
- **H1 only once per note**, heading levels must not skip (no H2 → H4)
- **Theme**: Minimal with Minimal Settings and Style Settings plugins

## Callout usage

| Callout type | When to use |
|-------------|-------------|
| `> [!note]` | Supplementary notes, concept explanations |
| `> [!tip]` | Practical tips, shortcuts |
| `> [!notice]` | Information worth noting |
| `> [!warning]` | Operations that may cause problems |
| `> [!important]` | Critical info, must read |
| `> [!info]` | Background knowledge, principles |

Use `> [!type]- Title` for collapsible callouts on long content.

## Observing existing conventions

When creating or editing notes:
- Existing notes all have a `> [!note] 相关笔记` callout listing related wikilinks near the top
- Task notes under `任务清单/` use `tags: [tasks/daily]` and link tasks to knowledge notes
- Every note that links to another should ensure the reverse link exists too

## Reference

For detailed markdown formatting conventions (inline formatting, block elements, Obsidian-specific syntax tables), see `CLAUDE.md`. If `AGENTS.md` and `CLAUDE.md` conflict, trust `CLAUDE.md` for content rules and `AGENTS.md` for tool/workflow rules.
