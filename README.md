# `nomad-docs`

This repository contains the documentation for the central NOMAD distribution.

## Contributing

To contribute, please open a pull request (PR) with your changes. **At least one review from a FAIRmat co-worker is required before merging**. If you are not sure who to assign, please ask in the PR conversation by tagging @ahm531 or @JFRudzinski.

## Running the Docs Server Locally

If you have a `nomad-dev-distro` setup, you can follow the [day to day development](https://github.com/FAIRmat-NFDI/nomad-distro-dev?tab=readme-ov-file#day-to-day-development) instructions to install `nomad-docs` as a submodule there.


If you have an up-to-date Python installation (3.11 or 3.12), see [Help to install Python](#help-to-install-python-311-or-312) below.

1. Upgrade pip and install uv

```sh
pip install --upgrade pip
pip install uv
```

2. Run the Local Docs Server

Once `uv` is installed, you can start the MkDocs server with:

```bash
uv run mkdocs serve
```

This will install all requirements in a virtual environment and start the local development server.

> 💡 **Tip:** To compare your local docs with the latest version once you start making significant changes, use the [DEV Deployment DOCS](https://nomad-lab.eu/prod/v1/develop/docs/index.html).

---

## Help to install Python 3.11 or 3.12

> **Note:** Replace `3.11` with `3.12` below if you prefer to use Python 3.12.

If Python 3.11 is not installed on your system, use the instructions below based on your OS:

**Debian Linux**

```bash
sudo apt install python3.11
```

**Red Hat Linux**

```bash
sudo dnf install python3.11
```

**macOS**

```bash
brew install python@3.11
```

**Windows PowerShell**

1. Download the installer from the [official Python website](https://www.python.org/downloads/release/python-3110/).
2. Run the installer.
3. Make sure to check the box **"Add Python 3.11 to PATH"** during installation.

