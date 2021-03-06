# Contributing to Notebooks

## Notebook Naming

Notebooks cannot have spaces in the paths. Use underscores instead. This is
because pytest does not like spaces in command line arguments.

## Docker Image Validation

The docker image must be able to run all notebooks. Whenever a change is made to
the docker image, it must be validated. This can be accomplished by automatically
running all of the notebooks using the supplied test script. This is accomplished
by running the notebook in interactive mode, achieved by adding `/bin/bash` to 
the container run command, e.g.
```bash
docker run -it --rm -p 8888:8888 -v $PWD:/home/jovyan/work -e PL_API_KEY='[YOUR-API-KEY]' planet-notebooks /bin/bash
```

From the root directory within the docker container, run one of the following:

1. To run all notebooks
    ```bash
    $> pytest tests/test_notebooks.py
    ```
1. run only notebooks in a subdirectory using
    ```bash
    $> pytest tests/test_notebooks.py --path <subdirectory>
    ```
1. run one or more notebooks (separated by spaces)
    ```bash
    $> pytest tests/test_notebooks.py --notebooks <notebook1> <notebook2> <...>
    ```
1. run only notebooks that have a given dependency, <package>
    ```bash
    $> pytest tests/test_notebooks.py --notebooks "$(grep -rl import <package> jupyter-notebooks/)"
   ```

## Automated Running and Skipping

To enable validation of the Docker image, every notebook should run successfully
when run from the command line. For notebooks where that just is not possible, 
the notebooks can be excluded from automated running by adding its path to 
`tests/skip_notebooks`. Excluding a notebook from automated running
means that it is excluded from Docker Image validation. **If a notebook is
skipped, it will not be guaranteed to be supported by the Docker image.**

Skipping of notebooks within `skip_notebooks` can be achieved with by adding the
`--no-skip` option to the pytest command.
