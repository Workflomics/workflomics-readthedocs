Domain Development
##################

This guide outlines the steps to add a domain to the Workflomics platform, and how to maintain the existing domains.

Create New Domain
*****************

The following steps describe how to create a new domain on the Workflomics platform. Creating a domain can be broken down into the following steps:

1. Cloning the Workflomics annotations repository
2. Configuring a new domain, including the domain configuration, constraints, and initial tool annotations.
3. Include tools with existing CWL annotations and link the domain tools to their CWL descriptions
4. Adding new tools and CWL descriptions that are currently missing in your domain
5. Testing the domain configuration using the APE library
6. Adding the domain to the Workflomics database

The following sections provide a detailed guide on how to create a new domain on the Workflomics platform.

Setup the Domain Description
============================


1. Fork the `workflomics/tools-and-domains <https://github.com/Workflomics/tools-and-domains>`_ repository on GitHub and clone the repository to your local machine.
2. Add a new domain folder under `domains <https://github.com/Workflomics/tools-and-domains/tree/main/domains>`_, e.g., `domains/my-domain`. The content could be copied from the `domains/template <https://github.com/Workflomics/tools-and-domains/tree/main/domains/template-domain>`_, which provides a template for the domain files.
   
   - The `domains/my-domain` folder should contain the following files (the file names do not have to be the same):
   
      .. code-block::

         my-domain/
         ├── tools.json
         ├── config.json
         └── constraints.json

   - **tools.json**: The file contains the bio.tools annotations for the tools in the domain. The `template` file contains tool annotations for the following tools: `Comet`, `PeptideProphet`, and `ProteinProphet`. Once you have a list of tools in mind, it is recommended to re-generate the `tools.json` file from existing bio.tools annotations using the APE CLI (Command Line Interface).The detailed guide on how to generate a new `tools.json` file can be found in the `APE CLI documentation <https://ape-framework.readthedocs.io/en/v2.4/docs/developers/cli.html#convert-tools>`_.
   - **config.json**: The file contains the domain-specific metadata and configuration.  The `template` should provide most of the fields already defined, such as `ontology_path`, `ontologyPrefixIRI`, etc. The fields that should be updated are: `tool_annotations_path` and `constraints_path` (as they currently point to the template folder). In addition, `inputs` and `outputs` should be updated to reflect the domain-specific input and output types. For more information on the configuration options, see the `configuration documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/domain.html#core-configuration>`_.
   - **constraints.json**: The file contains the domain-specific constraints. This file could be included in the `config.json` file, or linked from the `config.json` file (as in this template). The constraints file should contain the domain-specific constraints. For more information on the constraints file format, see the `constraints documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/constraints.html#constraint-templates>`_.
   
   
3. The tools specified in the `tools.json` should link to the CWL descriptions in the `tools-and-domains/cwl-tools <https://github.com/Workflomics/tools-and-domains/tree/main/cwl-tools>`_ directory. The `cwl` field should point to the CWL file in the raw file within `cwl-tools` repository. You can always use the `domains/template <https://github.com/Workflomics/tools-and-domains/tree/main/domains/template-domain>`_ or the `proteomics domain <https://github.com/Workflomics/tools-and-domains/blob/main/domains/proteomics/tools.json>`_ for reference. A snipped referencing the CWL fill is shown in the example below:

.. code-block::

   {
      "id": "Comet",
      "label": "Comet",
      "implementation" : { "cwl_reference" : "https://raw.githubusercontent.com/Workflomics/t/main/cwl-tools/Comet/Comet.cwl"} ,
      ...
   }
   
4. If the tools do not have a corresponding CWL description available, you should create the folders under `tools-and-domains/cwl-tools` and add the CWL files for the tools. The folders and CWL files should be named according to the tool name, e.g., `Comet/Comet.cwl`, `PeptideProphet/PeptideProphet.cwl`, and `XTandem/XTandem.cwl`. The CWL files should be annotated with the bio.tools annotations for inputs and outputs. The tool annotations in `tools.json` should be updated to point to the newly created CWL files. A detailed guide on how to create CWL files is available in :doc:`adding-tools`.
5. Once the domain is configured, it should be locally tested using APE library. The APE library can be used to validate the domain configuration and check for any errors. Tutorial on how to use the APE library can be found on the `My first APE run <https://ape-framework.readthedocs.io/en/latest/docs/basics/gettingstarted.html>`_. The configuration file used in the example should be the `config.json` file you created in the `domains/my-domain` folder.
6. If the domain configuration is correct, a pull request should be created to merge the changes made to the forked repository. The pull request should be reviewed by the Workflomics developers team.
7. Once the pull request is merged, the domain should be added to the Workflomics database. The domain is added by the Workflomics developers team, and initiated by the user who created the PR. The user should create an issue in the `Workflomics repository <https://github.com/Workflomics/workflomics-frontend/issues/new/choose>`_ to request the domain addition. The issue should contain the domain name and a brief description of the domain as well as the link to the configuration file - `config.json`. The instructions on how to add the domain to the database can be found in Section `Configure Workflomics <#configure-workflomics>`_.

.. _configure-cwl-files:

Configure CWL files
===================

The CWL files for the tools in the domain should be added to the `tools-and-domains/cwl-tools <https://github.com/Workflomics/tools-and-domains/tree/main/cwl-tools>`_ repository. Each tool should be annotated in a separate CWL file. The CWL files should be named according to the tool name, e.g., `comet.cwl`, `peptideprophet.cwl`, and `proteinprophet.cwl`. The CWL files should be annotated with the bio.tools annotations.

Once the CWL files are added to the repository, the domain should be updated to point to the correct CWL files. The CWL files should be linked in the `tools.json` file in the domain directory. The `cwl` field should point to the CWL file in the raw file within `cwl-tools` repository. You can always use the `domains/template <https://github.com/Workflomics/tools-and-domains/tree/main/domains/template-domain>`_ or the `proteomics domain <https://github.com/Workflomics/tools-and-domains/blob/main/domains/proteomics/tools.json>`_ for reference. A snipped referencing the CWL fill is shown in the example below:

.. code-block::

   {
      "id": "Comet",
      "label": "Comet",
      "implementation" : { "cwl_reference" : "https://raw.githubusercontent.com/Workflomics/tools-and-domains/main/cwl-tools/Comet/Comet.cwl"} ,
      ...
   }

Tutorial on how to create CWL files can be found on Elixir's `Training Platform <https://tess.elixir-europe.org/materials/cwl-user-guide>`_.

.. _configure-workflomics:

Configure Workflomics
=====================

Once the issue was created for adding a new domain to Workflomics, the new domain should be added to the Postgres database. The database can be updated from the `SQL script <https://github.com/Workflomics/workflomics-frontend/blob/main/database/03_import_data.sql>`_ that contains the domain information. 

Once the `script <https://github.com/Workflomics/workflomics-frontend/blob/main/database/03_import_data.sql>`_ is updated to include the new domain in the `public.domain` table, and merged into the `main` branch, the Workflomics server should be updated to reflect the new domain annotations. 

In case a user took the initiative of updating the `script`, please create a PR into the `main` and request a review from the Workflomics developers team. If you have any questions or need help, please contact the `Workflomics developers team <https://workflomics.readthedocs.io/en/domain-creation/#contributors>`_.

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


