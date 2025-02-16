# BPX

An implementation of the BPX battery parameter exchange format in Pydantic.

Features a Pydantic-based parser for json files in the BPX format, which validates your file against the schema.

## Prerequisites

- Python 3+

## Installation

Create a new virtual environment, or activate an existing one (this example uses the python `venv` module, but you could use Anaconda and a `conda` environment)

```bash
python3 -m venv env
source env/bin/activate
```

Install the `BPX` module from the repository on github

```bash
pip install git+https://github.com/pybamm-team/BPX.git
```

## Usage

Create a python script similar to that below

```python
import bpx

filename = 'path/to/my/file.json'
my_params = bpx.parse_bpx_file(filename)
```

`my_params` will now be of type `BPX`, which acts like a python dataclass with the same attributes as the BPX format. For example you can print out the initial temperature of the cell using

```python
print('Initial temperature of cell:', my_params.parameterisation.initial_temperature)
```

If you want to pretty print the entire object, you can use the `devtools` package to do this (remember to `pip install devtools`)

```python
from devtools import pprint
pprint(my_params)
```

You can convert any `Function` objects in your bpx to regular callable Python functions, for example:

```python
cathode_diffusivity = my_params.parameterisation.cathode.diffusivity.to_python_function()
diff_at_one = cathode_diffusivity(1.0)
print('cathode diffusivity at x = 1.0:', diff_at_one)
```

If you want to output the complete json schema in order to build a custom tool yourself, you can do so:

```python
print(bpx.BPX.schema_json(indent=2))
```

According to the `pydantic` docs, the generated schemas are compliant with the specifications: JSON Schema Core, JSON Schema Validation and OpenAPI.
