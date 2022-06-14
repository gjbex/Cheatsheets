# Automatically switch conda environments

Conda environments are very useful to provide project-specific version of
Python and Python packages.  However, if you work with many projects, it
can be inconvient to switch environments when you switch between projects.

The Linux tool `direnv` can help to automate this process.

## Configuration

### conda

Start with a typical miniconda install.  As part of the installation process,
you shell initialization, e.g., `.bashrc` is modified.  The code added
typically looks as follows:

```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/gjb/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/gjb/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/home/gjb/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/gjb/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

Copy this code snippet into a file in your home directory named `.conda_init.sh`.

**Note: this code snippet contains the path to your home directory, so do not
copy/paste the conda initialization code above.**

Next, replace the code snippent in your `.bashrc` by:

```bash
source ~/.conda_init.sh
```

### direnv

Install direnv using your package manager, e.g., for Ubuntu:
```bash
$ sudo apt install direnv
```

You need a configuration file for direnv that defines a "layout", i.e., a function
that can be called later to change your environment.  This configuration file is
`.direnvrc` in your home directory and should contain the following:

```bash
layout_conda() {
# Ref Url: https://github.com/direnv/direnv/wiki/Python
# local ANACONDA_HOME=”${HOME}/anaconda3"
# PATH_add “$ANACONDA_HOME”/bin
    source ~/.conda_init.sh
    if [ -n "$1" ]; then
# Explicit environment name from layout command.
        local env_name="$1"
        conda activate ${env_name}
    elif (grep -q name: environment.yml); then
# Detect environment name from `environment.yml` file in `.envrc` directory
        conda activate $(grep name: environment.yml | sed -e 's/name: //' | cut -d "'" -f 2 | cut -d '"' -f 2)
    else
        (>&2 echo No environment specified);
        exit 1;
    fi;
}
```

Note that if the directory you are change to (and that is a `direnv` controlled
directory, the `environment.yml` file will be used to determine the environment to
initialize.

### Project directory

In the project directory, create an `environment.yml` file that defines the
environment you want to use.

Next, create a file `.envrc` with the following contents:

```bash
layout conda
```

Make sure that the directory can be used with `direnv` by running the following
command in the project directory:

```bash
$ direnv allow
```

Now you are good to go.  When you change directories into the project directory,
conda environment will be activated automatically.
