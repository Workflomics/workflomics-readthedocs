Adding a new tool
=================

1. Find or create a docker image
--------------------------------

Search for the tool and the ways it is distributed. Some tools already provide a docker image that can be used to run it. Sometimes, third parties will provide a docker image, but in practice these are not consistently updated, so be sure to check the version of the tool. Other tools will be available in a "generic" image containing multiple tools. If no suitable image can be found, create one yourself that wraps the tool, taking into account its dependencies and licenses.

** TODO: Create a Dockerfile **

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


3. Create the CWL annotation
----------------------------

.. important::

   The initial template for the CWL file can be generated from existing bio.tools annotations using the `APE` command line interface. See the `APE pull-a-tool <https://ape-framework.readthedocs.io/en/v2.4/docs/developers/cli.html#>`_ documentation for more information. The generated CWL file annotates the expected inputs and outputs and should be used as a starting point and modified to fit the specific tool version and requirements.

These are the most important parts of the CWL file:

- ``baseCommand``: The command to run the tool. This can be a list of strings, where the first string is the command and the rest are arguments. In practice though, especially when there are pre- or post-processing steps, a shell command is used and the actual command is in the ``valueFrom`` field of an argument. This requires ``ShellCommandRequirement`` to be specified. Also, the arguments passed to the command refer to the inputs that are defined further on in the CWL file.
- ``DockerRequirement``: This is where the Docker image is defined. The ``dockerPull`` field should contain the name of the image, and the ``dockerOutputDirectory`` field should contain the directory where the output files are written to. The CWL runner will mount this directory to the host, so the files can be accessed after the run. Alternatively, if a docker image is not available, a Dockerfile can be specified inline in the ``dockerFile`` field. In that case, the docker image will be built locally before the tool is run.
- The inputs: specification of the inputs, including the type and format. The inputBinding field specifies how the input should be passed to the command.
- The outputs: specification of the outputs, including the type and format. The outputBinding field specifies how the output should be collected from the command, for instance by globbing a file.

.. code-block:: yaml

  cwlVersion: v1.2
  label: Sage
  class: CommandLineTool
  baseCommand: ["/bin/bash", "-c"]
  arguments:
    - valueFrom: >
        "/msamanda/MSAmanda -s $(inputs.MS_Amanda_in_1.path) \
        -d $(inputs.MS_Amanda_in_2.path) \
        -e $(inputs.Settings)"
      shellQuote: false
  requirements:
    ShellCommandRequirement: {}
    DockerRequirement:
      dockerPull: workflomics/msamanda:latest
      dockerOutputDirectory: /data/output

  inputs:
    MS_Amanda_in_1:
      type: File
      format: "http://edamontology.org/format_3244" # mzML
      inputBinding:
        position: 1
        prefix: -s
    MS_Amanda_in_2:
      type: File
      format: "http://edamontology.org/format_1929" # FASTA
      inputBinding:
        position: 2
        prefix: -d
    Settings:
      type: string
      default: /msamanda/settings.xml
      inputBinding:
        position: 3
        prefix: -e

  outputs:
    MS_Amanda_out_1:
      type: File
      format: "http://edamontology.org/format_3247" # mzIdentML
      outputBinding:
        glob: /data/output.mzid

The CWL file essentially describes one step from a workflow and we want to try whether it works as expected. The CWL file can be tested using the cwltool command line tool. For instance:


.. code-block:: bash

  cwltool --validate path/to/cwlfile.cwl



1. Create a workflow
--------------------

Create a workflow using the tool and test whether it runs.
