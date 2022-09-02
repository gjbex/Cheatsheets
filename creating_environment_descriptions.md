# conda environment descriptions

There are several ways to exchange/reproduce conda environments.

## Exact reproduciton

Use a specification file generated with
```bash
$ conda list --explicit > spec_file.txt
```

Best to ensure reproducibility.


## Portable environment

A more portable export of the environment can be created as well.
Versions of packages are listed, but no architecture specifics.
```bash
$ conda env export > environment.yml
```

Best compromise.

## Completely protable environments

It is also possible to export a specification with only the
packages that were install due to what the user specified, no
dependencies, no versions.
```bash
$ conda env export --from-history > environment.yml
````

Best to ensure portability.  However, note that packages installed
using `pip` are **not** included.

## Recreating the environment

To create an environment form a `.yml` file, use

```bash
$ conda env create -f environment.yml
```

To create an environment for a specification file, use

```bash
$ conda create -n <name> --file spec_file.txt
```
