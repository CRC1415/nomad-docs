<!-- markdownlint-disable MD041 -->

## Part 2: Parsing from an ELN entry

In this part you will create a parser using the ELN approach. On the one hand, more manual steps will need to be performed by the user for the file processing; on the other hand, the resulting entry will be user-editable, allowing users to overwrite or modify information provided by the source file.

From the user point of view, the NOMAD entry should be created manually, and some information can be filled in manually as well. Next, the source file can be added, and only the data corresponding to the **empty** fields of the entry (not filled by the user) will be filled from the source file. The manually filled quantities will remain unchanged, even if the data there contradicts the source file. After that, the entry remains editable and manual changes have priority over the source file.

From the developer point of view, this means that the actual parser entry point is not needed. The entry can be created from the schema, and all the necessary steps can be performed within the `normalize()` method of the schema.

### Tutorial instructions

!!! example "Tutorial 2.1"
    Please switch to the parser tutorial 2 by commenting lines corresponding to `tutorial_1` and uncommenting line corresponding to `tutorial_2` in the `[project.entry-points.'nomad.plugin']` section of the `pyproject.toml`.

    You can find this file in the `tutorial-mode` branch under the root of the repository. Read the instructions in the code for more information.

??? success "Tutorial 2.1: Solution"
    ```toml
    [project.entry-points.'nomad.plugin']
    # parser_tutorial_1_schema = "nomad_plugin_tutorials.parsers.tutorial_1.schema:microscopy"
    # parser_tutorial_1_parser = "nomad_plugin_tutorials.parsers.tutorial_1.parsers:microscopy"

    parser_tutorial_2_schema = "nomad_plugin_tutorials.parsers.tutorial_2.schema:microscopy"

    # parser_tutorial_3_schema = "nomad_plugin_tutorials.parsers.tutorial_3.schema:microscopy"
    # parser_tutorial_3_parser = "nomad_plugin_tutorials.parsers.tutorial_3.parsers:microscopy"
    ```

Note that the parser entry point is not present, and that the schema has already been created. Note also that all of the quantities defined in the `OpticalMicroscopy` schema and its subsections have an additional `a_eln` parameter set to:

```py
ELNAnnotation(component=ELNComponentEnum.<data type>
```

This makes the corresponding fields in the GUI user-editable (see more details in the [schema tutorial](create_schema_package.md)).

!!! example "Tutorial 2.2"

    The `normalize()` method of the `OpticalMicroscopy` class is currently only calling the same method of the parent class and then raising `NotImplementedError`. Please update `normalize()` method such that it:

    1. Checks if the `data_file` is present / not empty.

    2. If yes, uses the `data_file` value and the `read_data_file()` (same as in parser tutorial part 1) utility function to read the metadata file into a python dictionary.

    3. Then, if the dictionary is created successfully, calls a `write_data()` method (see Tutorial 2.3) that will fill the entry archive.

    You can find this class in the `tutorial-mode` branch under `src / nomad_plugin_tutorials / parsers / tutorial_2 / schema / schema_package.py`. Read the instructions in the code for more information.

??? success "Tutorial 2.2: Solution"
    ```py

    class OpticalMicroscopy(Measurement, EntryData):

    ...

        def normalize(self, archive: 'EntryArchive', logger: 'BoundLogger') -> None:
            """
            Redefining the `normalize` method to read the data from the provided file and
            populate other quantities of the schema. This method is called when the entry
            is processed.
            """

            super().normalize(archive, logger)

            data_dict = {}
            if self.data_file is not None:
                data_dict = read_data_file(self.data_file, archive, logger)
            if data_dict:
                self.write_data(data_dict, logger)
    ```

You have just used `write_data()` method that has not been implemented yet.

!!! example "Tutorial 2.3"

    Implement the parsing logic of the `write_data()` method of the `OpticalMicroscopy` class.

    The data from the file can be first filled into a temporary object of the same class as the entry archive (already created in the code):

    ```py
    measurement = OpticalMicroscopy()
    ```

    Note that a significant amount of the code from the parser in tutorial 1 can be reused here.

    You can find this class in the `tutorial-mode` branch under `src / nomad_plugin_tutorials / parsers / tutorial_2 / schema / schema_package.py`. Read the instructions in the code for more information.

??? success "Tutorial 2.3: Solution"

    ```py

    class OpticalMicroscopy(Measurement, EntryData):

    ...


    def write_data(self, data_dict: dict, logger: 'BoundLogger') -> None:
        
        ...

        measurement = OpticalMicroscopy()
        if datetime := data_dict.get('datetime'):
            measurement.datetime = datetime
        if (
            'sample' in data_dict
            and isinstance(data_dict['sample'], dict)
            and 'sample_ID' in data_dict['sample']
        ):
            measurement.m_setdefault('samples/0')
            measurement.samples[0].lab_id = data_dict['sample']['sample_ID']
            if 'description' in data_dict['sample']:
                measurement.description = data_dict['sample']['description']

        measurement.m_setdefault('settings')
        if resolution := data_dict.get('resolution'):
            measurement.settings.resolution = [float(x) for x in resolution.split('x')]
        if magnification := data_dict.get('magnification'):
            measurement.settings.magnification = float(magnification[:-1])

        measurement.m_setdefault('results/0')
        if image_file_name := data_dict.get('imageFileName'):
            measurement.results[0].image = os.path.join(
                os.path.dirname(self.data_file), image_file_name
            )
    ```

Now the data in the temporary object can be merged into the entry archive so that the priority is given to the data already in the entry archive. This can be done recursively for all subsections of a given archive.

!!! example "Tutorial 2.4"

    Use an example that is already present in the `utils.py` file of the plugin as the `merge_sections()` function to complete `write_data()` method of the `OpticalMicroscopy` class.

    You can find this class in the `tutorial-mode` branch under `src / nomad_plugin_tutorials / parsers / tutorial_2 / schema / schema_package.py`. Read the instructions in the code for more information.

??? success "Tutorial 2.4: Solution"
    ```py

    class OpticalMicroscopy(Measurement, EntryData):

    ...


    def write_data(self, data_dict: dict, logger: 'BoundLogger') -> None:
        
        ...

        merge_sections(self, measurement, logger=logger)
    ```
