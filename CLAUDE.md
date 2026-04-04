# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal technical documentation site built with [MkDocs](https://www.mkdocs.org/) using the [Material theme](https://squidfunk.github.io/mkdocs-material/). It is published to GitHub Pages at `https://fdmsantos.github.io/mytechdocs/`.

## Common Commands

### Python (local) — recommended

Dependencies are managed via `requirements.txt`. The venv is created automatically on first run.

```bash
make serve    # create venv (first time) and start live server at localhost:8000
make build    # build static site into site/
make deploy   # deploy to GitHub Pages
```

### Docker

```bash
make docker-run    # build image and start server
make docker-serve  # start server with existing image
```

## Architecture

- `mkdocs.yml` — site configuration: theme, navigation, markdown extensions, and plugins
- `docs/` — all content as Markdown files, mirroring the nav structure in `mkdocs.yml`

The navigation tree in `mkdocs.yml` under the `nav:` key is the authoritative structure. Adding a new page requires both creating the `.md` file under `docs/` and adding an entry to `nav:`.

### Plugins & Extensions

- **Material theme** with `navigation.tabs`, `navigation.sections`, and `toc.follow`
- **pymdownx** extensions: syntax highlighting (`superfences`, `highlight`), `snippets`, `smartsymbols`
- **glightbox** for image lightbox support
- TOC depth is set to 5 (`toc_depth: 5`)

### Content Areas

| Nav Section | Path |
|---|---|
| Kubernetes | `docs/kubernetes/` |
| DevOps / Monitoring | `docs/devops/monitoring/` |
| Cloud / Azure | `docs/cloud/azure/` |
| Data Engineering | `docs/dataengineering/` |
| Programming | `docs/programming/` |
| Unix OS | `docs/unix/` |
| Certifications | `docs/certifications/` |

Images are stored in `images/` subdirectories alongside the relevant Markdown files.