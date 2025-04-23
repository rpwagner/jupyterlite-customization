---
layout: post
title:  "Embedding a Jupyter notebook REPL"
date:   2025-04-28 12:34:51 -0700
categories: jupyter
---

This is a simple exmample of how to embed a [Jupyter REPL](https://jupyterlite.readthedocs.io/en/stable/quickstart/embed-repl.html) using an `iframe`.

## Built-In REPL

<iframe
  src="https://rpwagner.github.io/jupyterlite-customization/nextlevel/custom/repl/index.html?kernel=python&toolbar=1&code=import globus_sdk"
  width="100%"
  height="75%"
></iframe>

## Notes

Here’s a bunch of links to what I’ve got working in WebAssembly. I wanted to port all of the [Globus notebooks](https://github.com/globus/globus-jupyter-notebooks), but then I saw the dependency graphs for some of the other SDKs and called it a night.
- If you want to test the platform intro notebook, it’s in this JupyterLite site, along with some other example notebooks: https://rpwagner.github.io/jupyterlite-customization/nextlevel/custom/lab/index.html
- That site points to this Pyodide stack that built including extra packages (like the Globus SDK).
  - https://rpwagner.github.io/pyodide-build/console.html
- The Pyodide environment is being built out of this repository. It clones Pyodide 0.27.5 and then adds the extra packages (there’s a minor namespace conflict, I didn’t know that `pyodide-build` is the name of the Pyodide build tools).
  - https://github.com/rpwagner/pyodide-build/
- Here’s the YAML file I needed to write for to get the Globus SDK to build. Much easier than some of the others I’ve tackled. https://github.com/rpwagner/pyodide-build/blob/main/packages/globus-sdk/meta.yaml
- And this is the build and deploy GH action definition, modified from the Pyodide one. You can see where the Pyodide release is cloned and then the additional package definitions from my repo are layered on.
  - https://github.com/rpwagner/pyodide-build/blob/main/.github/workflows/deploy.yaml
- The JupyterLite site is built from this repo. Having this separation between Pyodide and the JupyterLite sites is nice. It means that once the Pyodide stack is good, it’s easy and quick to update the notebooks. This repo is a bit messy, I was experimenting with having a static Jekyll website up a level to handle documentation and authentication.
  - https://github.com/rpwagner/jupyterlite-customization
- This is the directory for the JupyterLite config. The notebooks and data are in the content directory. https://github.com/rpwagner/jupyterlite-customization/tree/main/nextlevel
- Here’s the JupyterLite config that points to the custom Pyodide build.
  -https://github.com/rpwagner/jupyterlite-customization/blob/main/nextlevel/jupyter-lite.json
- We expect support for pop-up transport login flow soon, and that’s something I’d like to add as a JupyterLab plugin so we can easily get token into the browser storage.
