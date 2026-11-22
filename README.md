# sas-design

Design work for the Suits & Sandals site (code lives in `../sas-site`).

| Folder | What lives there |
|--------|------------------|
| `elyx/` | The Elyx project (`elyx.json` marks its root). All `.elyx` files, tokens, components, screens and their assets. Open with `elyx elyx/`. |
| `prototypes/` | Coded prototypes and throwaway explorations, one folder per prototype. |
| `studio/` | Video work: OpenFilm films and every file made for them, one folder per film. Start or open one with `npx -y openfilm@latest open studio/<film-name>`. |

## Large files stay out

This repo is public and holds source only. `.gitignore` keeps out video, audio, raw and layered images, design sources, 3D, archives, renders (`renders/`, `exports/`), OpenFilm's `.film/` caches and dependency folders. Media those files point at lives outside git, so a fresh clone has the films and designs but not their footage or music.

A pre-commit hook also refuses any staged file over 5 MB, whatever its type. Enable it once per clone:

```sh
git config core.hooksPath .githooks
```

Raise the cap for one clone with `git config hooks.maxFileMb <n>`.
