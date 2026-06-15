## Part 1: Parsing data into static entry

In this tutorial we will create a parser that can parse a raw file and
create a NOMAD archive with the raw file as the mainfile. We will extend
the `MatchingParser` and overwrite its `parse` method. We will use the `parse` method to populate the data section of archive based on a custom
schema. Then, we will add the parser and schema as plugin entry points and test the parsing.

- The resulting archive can use a user defined EntryData schema, loaded into from a schema package.
- parser reads the raw file

### `MatchingParser` and its `parse` method

`MatchingParser` is an abstract class that is used to match different
properties of a raw file, for example, name, file content, mime type, etc.
Once matched, a NOMAD archive is created, and `parse` method is called
to populate it. `parse` method gives an interface to populate the
connected archive.

- extend the MatchingParser class and overwrite the parse method
- mainfile is available based on the absolute path. read it and create a data dictionary

### Populate entry with custom schema

- archive is available. Populate the data section with user defined schema
- tutorial

### Populate standardized sections

- can populate other sections of archive (results, workflows2) too, but the schema for these sections are standardized by NOMAD

### Output: Static entries

- resulting entry is static, i.e. no interaction from the users is possible.
