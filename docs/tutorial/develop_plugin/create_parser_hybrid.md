# Matching and creating an ELN entry (Hybrid Approach)

Users often prefer a hybrid approach, where the data is pre-filled automatically from the input files, but the subsequent manual edits are also saved in the entry. Both the original information and the edited values are saved in this approach.

This can be achieved by creating two entries during parsing. The first one is a non-editable "Raw Data File" entry, and the second one is the user-editable version with populated schema.

Realizing this approach requires multiple steps from the developer. To simplify the process, we created a specialized `ElnParserEntryPoint` class which redefines a standard `ParserEntryPoint`. The specific classes for both Raw Data File entry and the ELN can be selected by the developer.

## Tutorial instructions

!!! question "Tutorial 3.1"
    Please switch to the parser tutorial 3 by commenting lines corresponding to `tutorial_2` and uncommenting line corresponding to `tutorial_3` in the `[project.entry-points.'nomad.plugin']` section of the `pyproject.toml`.

    You can find this file in the `tutorial-mode` branch under the root of the repository. Read the instructions in the code for more information.

??? success "Tutorial 3.1: Solution"
    ```toml
    [project.entry-points.'nomad.plugin']
    # parser_tutorial_1_schema = "nomad_plugin_tutorials.parsers.tutorial_1.schema:microscopy"
    # parser_tutorial_1_parser = "nomad_plugin_tutorials.parsers.tutorial_1.parsers:microscopy"

    # parser_tutorial_2_schema = "nomad_plugin_tutorials.parsers.tutorial_2.schema:microscopy"

    parser_tutorial_3_schema = "nomad_plugin_tutorials.parsers.tutorial_3.schema:microscopy"
    parser_tutorial_3_parser = "nomad_plugin_tutorials.parsers.tutorial_3.parsers:microscopy"
    ```

When using `ElnParserEntryPoint`, you can choose to utilize default versions of both the Raw Data File entry and the ELN entry (`ElnParserRawFile` and `ElnParserSection` respectively). Alternatively, you can define a specialized schema classes inheriting from the default classes for one or both of them. In this tutorial, we use the default schema for the raw file entry and a custom schema for the ELN entry.

!!! question "Tutorial 3.2"
    Define the appropriate parser entry point in the `__init.py__` file of the parsers folder. Note that the `OpticalMicroscopy` schema and its normalization is almost exactly the same as in the previous tutorial, and therefore all the manual user updates in the ELN will be correctly saved to the entry archive. The only difference is the inheritance from `ElnParserSection` instead of usual `EntryData`.

    You can find the file in the `tutorial-mode` branch under `src / nomad_plugin_tutorials / parsers / tutorial_3 / parsers / __init__.py`. Read the instructions in the code for more information.

??? success "Tutorial 3.2: Solution"

    ```py
    from nomad.config.models.plugins import ElnParserEntryPoint

    microscopy = ElnParserEntryPoint(
        name='Parser Tutorial 3: Microscopy Parser',
        description='Microscopy parser entry point.',
        mainfile_name_re=r'.*\.xml$',
        eln_m_def='nomad_plugin_tutorials.parsers.tutorial_3.schema.schema_package.'
        'OpticalMicroscopy',
    )
    ```

## Testing the plugin

If you use `nomad-distro-dev`, all functionality of the plugin can be tested within GUI by restarting the `nomad-distro-dev`. For the stand-alone installation of the plugin, other options are available: parsing only with a command line, or parsing and normalization with a python script.

### Parsing only with a command line

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

### Parsing and normalization with a python script

You can use `parse()` and `normalize_all()` functions from `nomad.client`, for example:

```py
from nomad.client import normalize_all, parse

archive = parse('src/nomad_plugin_tutorials/parsers/data/om_example/metadata.xml')[0]

## print out or assert something from the parsed archive here

normalize_all(archive)

## print out or assert something from the parsed and normalized archive here

```
