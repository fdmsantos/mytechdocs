# mytechdocs

My Technical documentation in MkDocs, published at <https://fdmsantos.github.io/mytechdocs/>.

## Running locally

### Python (recommended)

Requires Python 3. The venv and dependencies are set up automatically on first run.

```bash
make serve   # starts live-reloading server at http://localhost:8000
make build   # builds static site into site/
make deploy  # deploys to GitHub Pages
```

### Docker

```bash
make docker-run    # builds image and starts server
make docker-serve  # starts server with existing image
```