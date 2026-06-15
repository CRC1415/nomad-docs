<!-- markdownlint-disable MD041 -->

## Part 2: Manual input priority

In this part you will create a parser using the ELN approach. On the one hand, more manual steps will need to be performed by the user for the file processing; on the other hand, the resulting entry will be user-editable, allowing users to overwrite or modify information provided by the source file.

From the user point of view, the NOMAD entry should be created manually, and some information can be filled in manually as well. Next, the source file can be added, and only the data corresponding to the **empty** fields of the entry (not filled by the user) will be filled from the source file. The manually filled quantities will remain unchanged, even if the data there contradicts the source file. After that, the entry remains editable and manual changes have priority over the source file.

From the developer point of view, this means that the actual parser entry point is not needed. The entry can be created from the schema, and all the necessary steps can be performed within the `normalize()` method of the schema.

### Tutorial instructions

Please switch to the parser tutorial 2 by commenting lines corresponding to `tutorial_1` and uncommenting line corresponding to `tutorial_2` in the `[project.entry-points.'nomad.plugin']` section of the [`pyproject.toml`](https://github.com/FAIRmat-NFDI/nomad-plugin-tutorials/blob/main/pyproject.toml#L120){:target="_blank" rel="noopener"} of the plugin.

Note that the parser entry point is not present, and that the schema has already been created. Note also that all of the quantities defined in the `OpticalMicroscopy` schema and its subsections have an additional `a_eln` parameter set to:

```py
ELNAnnotation(component=ELNComponentEnum.<data type>
```

This makes the corresponding fields in the GUI user-editable (see more details in the [schema tutorial](create_schema_package.md)).

**Step 1**

The `normalize()` method of the `OpticalMicroscopy` class is currently only calling the same method of the parent class and then raising `NotImplementedError`. Please add the code lines to:

1. Check if the `data_file` is present.

2. If yes, use the `data_file` value and the `read_data_file()` (same as in parser [tutorial part 1](create_parser_parser_only.md)) utility function to read the metadata file into a python dictionary.

3. Then, if the dictionary is created successfully, call a `write_data()` method (see step 2) that will fill the entry archive.

**Step 2**

Implement the `write_data()` method.

1. The data from the file can be first filled into a temporary object of the same class as the entry archive (already created in the code):

    ```py
    measurement = OpticalMicroscopy()
    ```

    Note that a significant amount of the code from the parser in tutorial 1 can be reused here.

2. Now the data in the temporary object can be merged into the entry archive so that the priority is given to the data already in the entry archive. This can be done recursively; use an example that is already present in the `utils.py` file of the plugin as the `merge_sections()` function.
