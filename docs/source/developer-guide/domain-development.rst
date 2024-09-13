Domain Development
##################

This guide outlines the steps to add a domain to the Workflomics platform, and how to maintain the existing domains.

Create New Domain
*****************

Setup the Domain Description
============================

.. This is the content of every domain
.. - `tools.json`: Contains the tool descriptions of the domain. See the [documentation](https://ape-framework.readthedocs.io/en/latest/docs/specifications/setup.html#tool-annotations) to learn more about `tools.json` file format.
.. - `config.json`: Contains the domain-specific parameters. See the [documentation](https://ape-framework.readthedocs.io/en/latest/docs/specifications/setup.html#configuration-file) to learn more about the configuration file.
.. - `constraints.json`: Contains the constraints for the domain. The file could be included in the `config.json` file, or linked from the `config.json` file (as in this template). See the [documentation](https://ape-framework.readthedocs.io/en/latest/docs/specifications/setup.html#configuration-file) to learn more about the constraints file format.

1. Fork the `workflomics/containers <https://github.com/Workflomics/containers>`_ repository on GitHub and add a new domain folder under `domains`, e.g., `domains/my-domain`. The content could be copied from the `domains/template <https://github.com/Workflomics/containers/tree/main/template>`_ .
2. The domain directory should contain the following files (the file names do not have to be the same):

   .. code-block::

      my-domain/
      ├── tools.json
      ├── config.json
      └── constraints.json

   1. `bio.tools.json`: This file contains the bio.tools annotations for the tools in the domain. The file contains tool annotations for the following tools: `Comet`, `PeptideProphet` and `ProteinProphet`.
   
3. Fetch bio.tools annotations using the [TODO: add instructions] interface of RESTful APE. The annotations can be modified [TODO: We need to decide what to do in that case?]
4. The tool annotations should point to CWL files (uploaded to `cwl tools <https://github.com/Workflomics/containers/tree/main/cwl/tools>`_).

5. Adjust the default APE configuration in the `config.json` file. The `template` should provide most of the fields already defined, such as `ontology_path`, . For more information on the configuration options, see the `APE documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/setup.html#core-configuration>`_.
   
6. Domain constraints - this is useful to explain (with examples if possible)

7. Define demo input types - we should decide how to handle benchmarks

8.  Define domain benchmarks - we should provide a standardised way to describe benchmarks.

9.  Test the domain configuration by running the APE server with the new domain.
   

Configure CWL files
===================

The CWL files for the tools in the domain should be added to the `containers/cwl-tools <https://github.com/Workflomics/containers/tree/main/cwl-tools>`_ repository. Each tool should be annotated in a separate CWL file. The CWL files should be named according to the tool name, e.g., `comet.cwl`, `peptideprophet.cwl`, and `proteinprophet.cwl`. The CWL files should be annotated with the bio.tools annotations.

Once the CWL files are added to the repository, the domain should be updated to point to the correct CWL files. The CWL files should be linked in the `tools.json` file in the domain directory. The `cwl` field should point to the CWL file in the raw file within `cwl-tools` repository. You can always use the `domains/template <https://github.com/Workflomics/containers/tree/main/template>`_ or the `proteomics domain <https://github.com/Workflomics/containers/blob/main/domains/proteomics/tools.json>`_ for reference. A snipped referencing the CWL fill is shown in the example below:

.. code-block::

   {
      "id": "Comet",
      "label": "Comet",
      "implementation" : { "cwl_reference" : "https://raw.githubusercontent.com/Workflomics/containers/main/cwl/tools/Comet/Comet.cwl"} ,
      ...
   }

Tutorial on how to create CWL files can be found on Elixir's `Training Platform <https://tess.elixir-europe.org/materials/cwl-user-guide>`_.

Configure Workflomics
=====================

The new domain should be added to the Postgres database on the Workflomics platform. 



Update Existing Domain
**********************

This section describes how to update an existing domain on the Workflomics platform. 

Update Domain Annotations
=========================

To update the domain annotations you can u


