## Part 3: Hybrid approach

Users often prefer a hybrid approach, where the data is pre-filled automatically from the input files, but the subsequent manual edits are also saved in the entry. Both the original information and the edited values are saved in this approach.

This can be achieved by creating two entries during parsing. The first one is a non-editable "Raw Data File" entry, and the second one is the user-edited version with populated schema.

Realizing this approach requires multiple steps from the developer. To simplify the process, we created a specialized `ElnParserEntryPoint` class which redefines a standard `ParserEntryPoint`. The specific classes for both Raw Data File entry and the ELN can be selected by the developer.

### Tutorial instructions

Please switch to the parser tutorial 3 by commenting lines corresponding to `tutorial_2` and uncommenting line corresponding to `tutorial_3` in the `[project.entry-points.'nomad.plugin']`  section of the [`pyproject.toml`](https://github.com/FAIRmat-NFDI/nomad-plugin-tutorials/blob/main/pyproject.toml#L120) of the plugin.

Define the appropriate parser entry point in the `__init.py__` file of the parsers folder. In this example, you can use the default Raw Data File entry and specify the `OpticalMicroscopy` schema for the newly created ELN. Note that the `OpticalMicroscopy` schema and its normalization is exactly the same as in the previous tutorial, and therefore all the manual edits will be correctly saved to the entry archive.

## Testing the plugin

If you use `nomad-distro-dev`, all functionality of the plugin can be tested within GUI by restarting the `nomad-distro-dev`. For the stand-alone installation of the plugin, other options are available: parsing only with a command line, or parsing and normalization with a python script.

**Parsing only with a command line**

The parsing can be tested at any moment using a built-in NOMAD command `parse`.

1. Ensure that all the requirements for the plugin are installed. If you use `uv` in a stand-alone installation, run

    ```sh
    uv sync --extra dev
    ```

2. Run the parser with a selected input file:

    ```sh
    uv run nomad parse PATH/TO/THE/SELECTED/INPUT/FILE
    ```

    The result will be shown in the terminal. You can also save it into some temporary file, for example:

    ```sh
    uv run nomad parse PATH/TO/THE/SELECTED/INPUT/FILE > result.json
    ```

**Parsing and normalization with a python script**

You can use `parse()` and `normalize_all()` functions from `nomad.client`, for example:

```py
from nomad.client import normalize_all, parse

archive = parse('src/nomad_plugin_tutorials/parsers/data/om_example/metadata.xml')[0]

## print out or assert something from the parsed archive here

normalize_all(archive)

## print out or assert something from the parsed and normalized archive here

```
