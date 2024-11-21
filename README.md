# rmagine

These docs are built with [MkDocs](https://mkdocs.org) and published to [uos.github.io/rmagine_docs](https://uos.github.io/rmagine_docs)

## Build locally

```
sudo apt install python3-pip
pip3 install mkdocs mkdocs-material
```

*Note: you may need to adapt your path `export PATH=$PATH:/home/<USER>/.local/bin` to access mkdocs binary*

### Build Static Files

```
mkdocs build --strict
```

### Live reloading server on :8000

```
mkdocs serve --strict
```

## Deploy to naturerobots.github.io/mbf_docs

Anything pushed to branch `deploy` will trigger a Github Action that builds the website and updates [uos.github.io/rmagine_docs](https://uos.github.io/rmagine_docs)