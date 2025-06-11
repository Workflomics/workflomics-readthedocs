Adding a new tool
=================

1. Find or create a docker image
--------------------------------

Search for the tool and the ways it is distributed. Some tools already provide a docker image that can be used to run it. Sometimes, third parties will provide a docker image, but in practice these are not consistently updated, so be sure to check the version of the tool. Other tools will be available in a "generic" image containing multiple tools. If no suitable image can be found, create one yourself that wraps the tool, taking into account its dependencies and licenses.

**Example Dockerfile: MS Amanda**

If no suitable image is found, create one. For example, to package MS Amanda:

.. code-block:: docker

  FROM ubuntu:latest

  # Install dependencies
  RUN apt-get update && apt-get install -y \
      libc6-dev \
      libgcc1 \
      libgssapi-krb5-2 \
      libicu-dev \
      libssl-dev \
      libstdc++6 \
      zlib1g \
      && rm -rf /var/lib/apt/lists/*

  # Set working directory
  WORKDIR /msamanda

  # Copy MS Amanda binaries (e.g., MSAmanda, dependent files)
  COPY ./bin /msamanda

  # Make MS Amanda executable
  RUN chmod +x /msamanda/MSAmanda

  # Optional entrypoint
  ENTRYPOINT ["/msamanda/MSAmanda"]

This example assumes the MS Amanda binary and dependencies are placed in a local `bin/` directory. See:
https://github.com/Workflomics/tools-and-domains/tree/main/cwl-tools/ms_amanda/docker


2. Run the tool inside the container
------------------------------------

Chances are high that the tool or container has a specific way to be called, so here are some tips and tricks to try and get it running.

To try whether a tool works inside a docker container, you can run a docker container interactively. For instance:

.. code-block:: bash

  docker run -it --rm \
    --entrypoint /bin/bash \
    --mount=type=bind,source=/repos/containers/cwl-tools/Sage/test/data,target=/data/ \
    sage:latest 

This will start a bash shell inside the container, where you can try to run the tool. It mounts the local directory to /data/ inside the container, so put required input files and configuration there. Note that CWL runners will explicitly mount the input files, but mounting the directory can be useful for debugging.

Make sure the tool executes correctly and produces the expected output. Sometimes, additional config files will be required or arguments need to be passed in a specific way. If things are not working, consider the following:

- Check the version of the tool (usually with ``tool --version``).
- Check the help of the tool (usually with ``tool --help``)

It will be helpful to put the above command in a script, so you only need to figure out the correct command once. When the tool finally produces the expected output at the desired location, you are ready to go to the next step.


3. Create a semantically annotated CWL file
-------------------------------------------

.. important::

   The initial template for the CWL file can be generated from existing bio.tools annotations using the `APE` command line interface. See the `APE pull-a-tool <https://ape-framework.readthedocs.io/en/v2.4/docs/developers/cli.html#>`_ documentation for more information. The generated CWL file annotates the expected inputs and outputs and should be used as a starting point and modified to fit the specific tool version and requirements.

 Common Workflow Language (CWL) file formally describes how to run a computational tool. At minimum, a CWL file must clearly specify:

- **baseCommand**: The executable or command-line invocation.
- **inputs**: Files or parameters needed by the tool, including type and format.
- **outputs**: Result files produced by the tool, including type and retrieval method.

Additionally, CWL often specifies:

- **DockerRequirement**: A Docker image (`dockerPull`) containing the tool and its dependencies, and a directory for output (`dockerOutputDirectory`).
- **ShellCommandRequirement**: Enables complex shell commands within CWL (`valueFrom`).

In our approach, we extend basic CWL with semantic annotations to facilitate automated workflow composition. We incorporate the EDAM ontology to specify the computational purpose (`intent`), data types, and formats clearly and consistently. Specifically, we add:

- **intent**: EDAM `operation` terms (e.g., peptide identification) explicitly describing the tool's computational function.
- **Data types**: EDAM `data` terms next to each input/output, starting from a general root (`edam:data_0006`) refined into specific types.
- **Data formats**: EDAM `format` annotations precisely identifying file formats.

To keep annotations concise, we declare an EDAM namespace prefix under `$namespaces`.

This file can be automatically generated from the `bio.tools` annotations using the `APE pull-a-tool <https://ape-framework.readthedocs.io/en/v2.4/docs/developers/cli.html#>`_ command line interface (e.g, `java -jar APE-2.5.2-executable.jar pull-a-tool Sage-proteomics`). The generated file will contain the basic structure and annotations, which can then be modified to fit the specific tool version and requirements.

Here's a complete annotated example for the `Sage` tool, which performs peptide identification and retention time prediction:

.. code-block:: yaml

  cwlVersion: v1.2
  label: Sage-proteomics
  class: CommandLineTool
  baseCommand: ["/bin/bash", "-c"]
  arguments:
    - valueFrom: >
        "sage -o /data/output -f $(inputs.Sage_in_2.path) \
        $(inputs.Configuration.path) $(inputs.Sage_in_1.path) && \
        /data/sage_TSV_to_mzIdentML.sh /data/output/results.sage.tsv"
      shellQuote: false
  requirements:
    ShellCommandRequirement: {}
    DockerRequirement:
      dockerPull: workflomics/sage:latest
      dockerOutputDirectory: /data
    InitialWorkDirRequirement:
      listing:
        - class: File
          location: sage_TSV_to_mzIdentML.sh
          basename: sage_TSV_to_mzIdentML.sh

  $namespaces:
    edam: http://edamontology.org/

  intent:
    - http://edamontology.org/operation_3631  # Peptide identification
    - http://edamontology.org/operation_3633  # Retention time prediction
    - http://edamontology.org/operation_2428  # Validation

  inputs:
    Sage_in_1:
      type: File
      format: edam:format_3244  # mzML
      edam:data_0006: edam:data_0943  # Mass spectrum
    Sage_in_2:
      type: File
      format: edam:format_1929  # FASTA
      edam:data_0006: edam:data_2976  # Protein sequence

    Configuration:
      type: File
      format: edam:format_3464  # JSON
      default:
        class: File
        format: edam:format_3464  # JSON
        location: https://raw.githubusercontent.com/Workflomics/tools-and-domains/main/cwl-tools/Sage-proteomics/config.json

  outputs:
    Sage_out_1:
      type: File
      format: edam:format_3247  # mzIdentML
      edam:data_0006: edam:data_0945  # Peptide identification
      outputBinding:
        glob: /data/output/results.sage.mzid


The CWL file essentially describes one step from a workflow and we want to try whether it works as expected. The CWL file can be tested using the cwltool command line tool. For instance:


.. code-block:: bash

  cwltool --validate path/to/cwlfile.cwl


Adding a library as a tool
==========================

Sometimes a tool is not a standalone executable, but a library for a programming language. In this case, the tool can be wrapped in a script that calls the library. These can be R, Python, Java, or any other language. The script should be able to run the library with the correct arguments and produce the expected output. The script can be run in a docker container that contains the required library, environment, and dependencies. The CWL file should then call the script in the same way as a standalone executable.


Creating an R-based tool
-------------------------

1. Create the executable R script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Since many R packages only provide function calls, write a simple R script(e.g., run_mytool.R)
that accepts command-line arguments(via commandArgs(trailingOnly = TRUE)) and then calls the package's functions. 
Set this script as an executable in the Dockerfile and optionally specify it under ENTRYPOINT.

2. Pick a base image
~~~~~~~~~~~~~~~~~~~~

A common choice is the rocker family(e.g., rocker/r-base:4.2.0), which ensures a functional 
R environment. 

First, we suggest finding containers in biocontainers or docker hub. If there is no container for 
your tool, creating a dockerfile is needed. In your Dockerfile, use ``apt-get install`` for 
system libraries(e.g., libxml2-dev) and ``R -e"install.packages(...)"`` or ``BiocManager::install(...)`` 
for R packages.

3. Test the tool
~~~~~~~~~~~~~~~~

Launch the container in interactive mode by ``docker run -it ...`` to ensure the R script 
runs correctly and that all libraries are installed. 

4. Write the CWL file
~~~~~~~~~~~~~~~~~~~~~

In the `` baseCommand``, refer to ["Rscript", "/path/to/run_script.R"]. Define your inputs 
and outputs according to the script's parameters. 