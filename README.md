# Workflomics Documentation

[![Documentation Status](https://readthedocs.org/projects/workflomics/badge/?version=latest)](https://workflomics.readthedocs.io/en/latest/?badge=latest)

The repository is used to provide detailed documentation on how to use the Workflomics [web interface](https://github.com/Workflomics/workflomics-frontend) and its [ecosystem](https://github.com/Workflomics).

This GitHub template includes fictional Python library
with some basic Sphinx docs.

Read the tutorial here:

https://docs.readthedocs.io/en/stable/tutorial/

## How to build the documentation locally

1. Clone the repository and navigate to the root directory

2. Install the required packages

    ```bash
    pip install -r docs/requirements.txt
    ```

3. Build the documentation

    ```bash
    cd docs
    make html SPHINXOPTS="-W"
    ```

4. The documentation will be available in the `docs/build/html` directory. Open the `index.html` file in your browser to view the documentation.
