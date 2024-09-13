Domain Development
##################

This guide outlines the steps to add a domain to the Workflomics platform, and how to maintain the existing domains.

Create New Domain
*****************

The following steps describe how to create a new domain on the Workflomics platform. Creating a domain can be broken down into the following steps:

1. Cloning the Workflomics annotations repository
2. Configuring a new domain, including the domain description, constraints, and tool annotations.
3. Linking the domain tools to their CWL descriptions
4. Adding CWL descriptions that are currently missing in your domain and updating the domain configuration to point to the correct CWL files
5. Testing the domain configuration using the APE library
6. Adding the domain to the Workflomics database

The following sections provide a detailed guide on how to create a new domain on the Workflomics platform.

Setup the Domain Description
============================


1. Fork the `workflomics/containers <https://github.com/Workflomics/containers>`_ repository on GitHub. 
2. Add a new domain folder under `domains <https://github.com/Workflomics/containers/tree/main/domains>`_`, e.g., `domains/my-domain`. The content could be copied from the `domains/template <https://github.com/Workflomics/containers/tree/main/template>`_, which provides a template for the domain files.
   1. The `domains/my-domain` folder should contain the following files (the file names do not have to be the same):

   .. code-block::

      my-domain/
      ├── tools.json
      ├── config.json
      └── constraints.json
   2. `bio.tools.json`: The file contains the bio.tools annotations for the tools in the domain. The file contains tool annotations for the following tools: `Comet`, `PeptideProphet` and `ProteinProphet`. The file can be generated from `bio.tools <https://bio.tools>`_ annotations using the `APE RESTful API <https://ape-framework.readthedocs.io/en/latest/docs/restful-ape/introduction.html>`_. A RESTful API's POST endpoint `/fetch-biotools-annotations` expects a list of tool names and returns APE-compatible tool annotations.
   3. `config.json`: The file contains the domain-specific metadata and configuration.  The `template` should provide most of the fields already defined, such as `ontology_path`, `ontologyPrefixIRI`, etc. The fields that should be updated are: `tool_annotations_path` and `constraints_path` (as they currently point to the template folder). In addition, `inputs` and `outputs` should be updated to reflect the domain-specific input and output types. For more information on the configuration options, `APE documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/domain.html#core-configuration>`_.
   4. `constraints.json`: The file contains the domain-specific constraints. This file could be included in the `config.json` file, or linked from the `config.json` file (as in this template). The constraints file should contain the domain-specific constraints. For more information on the constraints file format, see the `APE documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/constraints.html#constraint-templates>`_.
   
3. The tools specified in the `tools.json` should link to the CWL descriptions in the `containers/cwl-tools <https://github.com/Workflomics/containers/tree/main/cwl-tools>`_ directory. The `cwl` field should point to the CWL file in the raw file within `cwl-tools` repository. You can always use the domains/template <https://github.com/Workflomics/containers/tree/main/template>`_ or the `proteomics domain <https://github.com/Workflomics/containers/blob/main/domains/proteomics/tools.json>`_ for reference. A snipped referencing the CWL fill is shown in the example below:

.. code-block::

   {
      "id": "Comet",
      "label": "Comet",
      "implementation" : { "cwl_reference" : "https://raw.githubusercontent.com/Workflomics/containers/main/cwl/tools/Comet/Comet.cwl"} ,
      ...
   }
   
4. If the tools do not have a corresponding CWL description available, you should create the folders under `containers/cwl-tools` and add the CWL files for the tools. The folders and CWL files should be named according to the tool name, e.g., `Comet/Comet.cwl`, `PeptideProphet/PeptideProphet.cwl`, and `XTandem/XTandem.cwl`. The CWL files should be annotated with the bio.tools annotations for inputs and outputs. The tool annotations in `tools.json` should be updated to point to the newly created CWL files. A detailed guide on how to create CWL files is available in Section `Configure CWL files <#configure-cwl-files>`_.
5. Once the domain is configured, it should be locally tested using APE library. The APE library can be used to validate the domain configuration and check for any errors. Tutorial on how to use the APE library can be found on the `My first APE run <https://ape-framework.readthedocs.io/en/latest/docs/basics/gettingstarted.html>`_. The configuration file used in the example should be the `config.json` file you created in the `domains/my-domain` folder.
6. If the domain configuration is correct, the domain should be added to the Workflomics database. The domain should be added to the Postgres database on the Workflomics platform. The instructions on how to add the domain to the database can be found in Section `Configure Workflomics <#configure-workflomics>`_.

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

The new domain should be added to the Postgres database on the Workflomics platform. The database can be updated from the `SQL script <https://github.com/Workflomics/workflomics-frontend/blob/main/database/03_import_data.sql>`_ available on the Workflomics platform. 

Once the `script <https://github.com/Workflomics/workflomics-frontend/blob/main/database/03_import_data.sql>`_ is updated and the new domain is added to the `public.domain` table, the Workflomics platform should be updated to reflect the new domain. Please contact the `Workflomics developers team <https://workflomics.readthedocs.io/en/domain-creation/#contents>`_ to update the platform.

An administrator should be able to update the Workflomics platform to reflect the new domain. The new domain should be visible on the Workflomics platform, and the tools in the domain should be available for use in the workflow editor.


Update Existing Domain
**********************

This section describes how to update an existing domain on the Workflomics platform. 
We distinguish between few types of updates:

1. Adding a new tool to the domain
2. Updating an existing tool in the domain
3. Adding domain specific constraints

Writing in progress.

Add New Tool
============

To update the domain annotations you can u


