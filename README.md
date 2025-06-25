## `nomad-docs`

This repository contains the documentation for the central NOMAD distribution.

### Contributing

To contribute, please open a pull request (PR) with your changes. **At least one review from a FAIRmat co-worker is required before merging**. If you are not sure who to assign, please ask in the PR conversation by tagging @ahm531 or @JFRudzinski.

###

### Running the Docs Server Locally

If you have a [nomad-dev-distro](https://github.com/FAIRmat-NFDI/nomad-distro-dev?tab=readme-ov-file#day-to-day-development) setup, you can of course install `nomad-docs` there.

> **Note:** Replace `3.11` with `3.12` below if you prefer to use Python 3.12.

#### 0. Install Python 3.11 (if needed)

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

---

#### 1. Create a Virtual Environment

Open a terminal and create a virtual environment using **Python 3.11**:

**macOS and Linux**

```bash
python3.11 -m venv .pyenv
```

**Windows PowerShell**

```powershell
py -3.11 -m venv .pyenv
```

---

#### 3. Activate the Virtual Environment

**macOS and Linux**

```bash
. .pyenv/bin/activate
```

**Windows PowerShell**

```powershell
.pyenv\Scripts\activate
```

---

#### 4. Upgrade pip and install uv (recommended)

```sh
pip install --upgrade pip
pip install uv
```

#### 5. Install `nomad-docs` in editable mode

```sh
pip install -e .
```

use `pip install -e .[dev]` instead to run the tests locally.

#### 6. Launch the docs

```bash
uv run mkdocs serve
```

This will start a local server where you can preview your changes.


> 💡 **Tip:** To compare your local docs with the latest version once you start making significant changes, use the [DEV Deployment DOCS](https://nomad-lab.eu/prod/v1/develop/docs/index.html).
