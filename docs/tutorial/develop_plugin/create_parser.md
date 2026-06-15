# Create a parser

This tutorial guides you through creating a NOMAD plugin with parsers for processing raw files into structured NOMAD entries.

## Before you begin

This tutorial assumes basic familiarity with Python and Git. You should have Python 3.10 or higher installed, along with Git. We recommend using a modern Python IDE (such as VS Code or PyCharm) to follow along.

We will use the [nomad-plugin-tutorials](https://github.com/FAIRmat-NFDI/nomad-plugin-tutorials){:target="_blank" rel="noopener"} repository to build the schema package step by step. Start by cloning the repository and navigating into it:

```bash
git clone git@github.com:FAIRmat-NFDI/nomad-plugin-tutorials.git
cd nomad-plugin-tutorials
```

To access the "tutorial mode" version of the code (which contains code-along exercises with missing snippets for you to implement), switch to the `tutorial-mode` branch:

```bash
git checkout tutorial-mode
```

For instructions on installing and running the plugin locally, refer to the repository's [README.md](https://github.com/FAIRmat-NFDI/nomad-plugin-tutorials#getting-started){:target="_blank" rel="noopener"}.

The parser tutorial code is located in the `src / nomad_plugin_tutorials / parsers` directory. It contains three sub-tutorials in
the directories `tutorial_1/`, `tutorial_2/`, and `tutorial_3/` corresponding to the 3 parts of parser tutorial.

--8<-- "docs/tutorial/develop_plugin/create_parser_parser_only.md"

--8<-- "docs/tutorial/develop_plugin/create_parser_eln_only.md"

--8<-- "docs/tutorial/develop_plugin/create_parser_hybrid.md"
