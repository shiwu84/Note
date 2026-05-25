# AGENTS.md

## Repo type

This is an **Obsidian vault**. When editing notes (`.md`), use the `obsidian-markdown` skill. For `.canvas` files use `json-canvas`, for `.base` files use `obsidian-bases`.

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

## Files never to commit

These are in `.gitignore` and must be left alone:

- `.obsidian/workspace.json` — personal workspace state, changes constantly
- `.obsidian/workspace-mobile.json` — mobile workspace state
- `.obsidian/plugins/*/main.js`, `manifest.json`, `styles.css` — plugin binaries, re-downloadable

## Obsidian config facts

- **Wikilinks use absolute paths**: `[[Linux/KVM]]`, `[[Linux/KVM#标题]]`, `[[Linux/KVM|别名]]`
- **`alwaysUpdateLinks: true`** — if you rename or move a note, Obsidian auto-updates incoming links
- **Attachment storage**: custom plugin stores attachments in `./assets/<NoteName>/`
- **Frontmatter required** on every note: `date: YYYY-MM-DD` and `tags` (at least 2, hierarchical: `tag/subtag`)
- **Notes are written in Chinese**
- **H1 only once per note**, heading levels must not skip (no H2 → H4)

## Observing existing conventions

When creating or editing notes:
- Existing notes all have a `> [!note] 相关笔记` callout listing related wikilinks near the top
- Task notes under `任务清单/` use `tags: [tasks/daily]` and link tasks to knowledge notes
- Every note that links to another should ensure the reverse link exists too

## Reference

For detailed markdown formatting conventions (callout types, table usage, bidirectional link policy, optimization workflow), see `CLAUDE.md`. If `AGENTS.md` and `CLAUDE.md` conflict, trust `CLAUDE.md` for content rules and `AGENTS.md` for tool/workflow rules.
