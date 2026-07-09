# tRIBS documentation
Repository for tRIBS documentation at ReadTheDocs.io. The current documentation is tailored to version 6.0.0 (08/2026), released under the MIT Licence. Copyright 2026 tRIBS Developers.

Link to latest tRIBS Read the Docs can be found [here.](https://tribshms.readthedocs.io/en/latest/)

## Building the docs locally

```bash
pip install -r docs/requirements.txt
cd docs
sphinx-build -b html . _build/html
```

Then open `docs/_build/html/index.html` in a browser.
