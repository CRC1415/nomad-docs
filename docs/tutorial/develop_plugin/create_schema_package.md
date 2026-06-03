# Create a Schema Package

This tutorial guides you through creating a NOMAD plugin with a custom schema package.
Schema can be added to NOMAD using YAML files, but this tutorial focuses on Python-based
schema packages, that are versioned using *git*, provide system-level integration with
NOMAD instance, and allows adding custom normalization logic to the schema.

## Learning Outcomes

By the end of this tutorial, you will be able to:

1. Create a NOMAD plugin with a custom schema package
2. Use NOMAD's Python API to define and register data schema
3. Add ELN capabilities to a schema package using special annotations
4. Add data processing functionality to a schema using normalization framework

## Before you begin

This tutorial assumes basic familiarity with Python and Git. You should have Python 3.9
or higher installed on your machine, along with Git for version control. We suggest
using a modern Python IDE (e.g., VSCode, PyCharm) to follow along with the tutorial,
but you can also use a text editor and command line if you prefer.

We will be using the
[nomad-parser-tutorial-exercise](https://github.com/FAIRmat-NFDI/nomad-parser-tutorial-exercise)
repository to build a simple schema package step by step. Start with cloning and
changing into the repository to your local machine:

```bash
git clone git@github.com:FAIRmat-NFDI/nomad-parser-tutorial-exercise.git
cd nomad-parser-tutorial-exercise
```

## Write a Python schema package

In this step, you will add a custom schema package to the plugin and make it available to NOMAD by converting an existing YAML-based schema into Python classes and registering it as part of the plugin.

??? example "Download the YAML schema used in this step"
    This step uses a YAML-based schema package that defines the structure of the sintering process.

    1. [Download `sintering.archive.yaml`](https://github.com/FAIRmat-NFDI/AreaA-Examples/blob/main/tutorial13/part3/files/sintering.archive.yaml){:target="_blank" rel="noopener"}.
    2. Save the file in your working directory.

    Alternatively, retrieve the file using the following `curl` command:
    ```sh
    curl -L -o sintering.archive.yaml "https://raw.githubusercontent.com/FAIRmat-NFDI/AreaA-Examples/main/tutorial13/part3/files/sintering.archive.yaml"
    ```
    
<!-- TODO: Create a directly downloadable file-->
??? info "Schema packages can also be written directly in Python."
    For step-by-step guidance on defining schema packages from scratch, see [How-to guide: Define NOMAD schema packages](../../howto/plugins/types/schema_packages.md)

### Generate schema classes

You will now use an external package `metainfo-yaml2py` to convert the yaml schema package
into python class definitions.

Install the package:

```sh
pip install metainfoyaml2py
```

Generate the schema classes from the `sintering.archive.yaml` file and place them in the `schema_packages` directory, by running the `metainfo-yaml2py` command.

```sh
metainfo-yaml2py sintering.archive.yaml -o src/nomad_sintering/schema_packages -n
```

The `-n` flag adds `normalize()` functions (will be used below), while the `-o` flag specifies the output directory.



## Add ELN capabilities to the schema package

Add input file support to the schema

Add a new `Quantity` to the `Sintering` class to reference the recipe file:

```py
data_file = Quantity(
    type=str,
    description='The recipe file for the sintering process.',
    a_eln={
        "component": "FileEditQuantity",
    },
)
```

The `a_eln` annotation configures the quantity to accept file uploads in the NOMAD GUI using the `FileEditQuantity` component.




## Add schema normalization logic

In this step, you add normalization process to the schema by implementing a `normalize()` method.
Normalization allows schema sections to derive structured values programmatically using Python.

??? example "Example input file used for normalization"
    This example uses a simple CSV recipe file that describes a sintering process.
    Each row represents a processing step and will be converted into a corresponding
    `TemperatureRamp` section during normalization.

    1. [Download `sintering_example.csv`](https://github.com/FAIRmat-NFDI/AreaA-Examples/blob/main/tutorial13/part3/files/sintering_example.csv){:target="_blank" rel="noopener"}.
    2. Save the file in your working directory.

    Alternatively, retrieve the file using the following `curl` command:
    ```sh
    curl -L -o tests/data/sintering_example.csv "https://raw.githubusercontent.com/FAIRmat-NFDI/AreaA-Examples/main/tutorial13/part3/files/sintering_example.csv"
    ```
<!-- TODO: Provide a direct download link -->

Next, implement the normalize() method to read the input file and populate the schema programmatically.

Implement the normalization process as follows:

1. Check if the data file is provided using  `if self.data_file`, if so, open it via `archive.m_context.raw_file()` method and read it with `pd.read_csv(file)`:

    ```py
    if self.data_file:
    with archive.m_context.raw_file(self.data_file) as file:
        df = pd.read_csv(file)
    ```

2. Create a list of processing steps by iterating over the data frame and instantiating `TemperatureRamp` section:

    ```py
        steps = []
        for i, row in df.iterrows():
        step = TemperatureRamp()
        step.name = row['step name']
        step.duration = ureg.Quantity(float(row['duration [min]']), 'min')
        step.initial_temperature = ureg.Quantity(row['initial temperature [C]'], 'celsius')
        step.final_temperature = ureg.Quantity(row['final temperature [C]'], 'celsius')
        steps.append(step)
    ```

    The code snippet above uses the NOMAD unit registry to handle all the units.

3. Assign the generated list to `self.steps`:

    ```py
    self.steps = steps
    ```

4. Add the required imports of pandas and the NOMAD unit registry to the top of `sintering.py` file: <!-- TODO: this file was not introduced before - calrify in the steps when it should be created -->

    ```py
    from nomad.units import ureg
    import pandas as pd
    ```

!!! success "Complete normalize implementation"

    ```py
      from nomad.units import ureg
      import pandas as pd


      class Sintering(Process, EntryData, ArchiveSection):
          '''
          Class autogenerated from yaml schema.
          '''
          m_def = Section()
          steps = SubSection(
              section_def=TemperatureRamp,
              repeats=True,
          )
          data_file = Quantity(
              type=str,
              description='The recipe file for the sintering process.',
              a_eln={
                  "component": "FileEditQuantity",
              },
          )
          def normalize(self, archive, logger: 'BoundLogger') -> None:
              '''
              The normalizer for the `Sintering` class.

              Args:
                  archive (EntryArchive): The archive containing the section that is being
                  normalized.
                  logger (BoundLogger): A structlog logger.
              '''
              super().normalize(archive, logger)
              if self.data_file:
                  with archive.m_context.raw_file(self.data_file) as file:
                      df = pd.read_csv(file)
                  steps = []
                  for i, row in df.iterrows():
                      step = TemperatureRamp()
                      step.name = row['step name']
                      step.duration = ureg.Quantity(float(row['duration [min]']), 'min')
                      step.initial_temperature = ureg.Quantity(row['initial temperature [C]'], 'celsius')
                      step.final_temperature = ureg.Quantity(row['final temperature [C]'], 'celsius')
                      steps.append(step)
                  self.steps = steps

    ```

## Register the schema package

!!! info "Why registering the schema package is required"
    Registering the schema package as a plugin entry point makes it discoverable by NOMAD at runtime. Without this registration, NOMAD cannot load the schema package, and the defined sections will not be available during data parsing or normalization.

Register the newly generated schema package as a plugin entry point by updating the metadata defined in the `__init__.py` file.

Copy the example `SchemaPackageEntryPoint` provided by the cookiecutter template and update:

1. The entry point class name
2. The import path in the `load()` method
3. The instance name and referenced class
4. The entry point name and description

For example:

```py
class SinteringEntryPoint(SchemaPackageEntryPoint):

    def load(self):
        from nomad_sintering.schema_packages.sintering import m_package

        return m_package


sintering = SinteringEntryPoint(
    name='Sintering',
    description='Schema package for describing a sintering process.',
)
```

Add the corresponding entry point to the `pyproject.toml` file.
Use the existing example at the bottom of the file as a template and update it to match the name of your entry point.

```toml
sintering = "nomad_sintering.schema_packages:sintering"
```

Reinstall the plugin to make the new entry point available:

```sh
pip install -e '.[dev]' --index-url https://gitlab.mpcdf.mpg.de/api/v4/projects/2187/packages/pypi/simple
```

Before you continue, commit your changes to git:

```sh
git add -A
git commit -m "Added sintering classes from yaml schema"
git push
```

## Check code formatting

The repository uses `Ruff` to enforce consistent code formatting and linting.  
Automatically generated files (for example from `metainfo-yaml2py`) may not fully comply with these rules, which can cause the formatting check in the GitHub Actions workflow to fail.

If you check the **Actions** tab of the GitHub repository, you might see that the last commit caused an error in the Ruff format check. To resolve this, check and format the code using Ruff.

Run the following command to check the code:

```sh
ruff check .
```

Apply automatic fixes if any issues are reported:

```sh
ruff check . --fix
```

Commit the formatting changes:

```sh
git add -A
git commit -m "Ruff linting"
git push
```

## Test on GUI

## Next steps
