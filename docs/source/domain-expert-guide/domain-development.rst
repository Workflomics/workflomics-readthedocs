Domain Development Guide
########################

This guide explains how to create and maintain a domain within the Workflomics platform. By following this step-by-step guide, you will learn how to configure a new domain, link it to CWL tool descriptions, and validate it using the APE library. Each step is broken down into detailed instructions for ease of use.

Overview of Steps
=================

1. **Clone the Workflomics Repository:** Set up your local environment by cloning the necessary repository.
2. **Create a New Domain Folder:** Organize your new domain directory with the required configuration files.
3. **Add Tools to Your Domain:** Include tools in your domain by reusing existing annotations or creating new ones.
4. **Configure Domain Files:** Update the domain configuration files with metadata, inputs, outputs, and constraints.
5. **Validate the Domain with APE Library:** Check your domain configuration for errors using the APE library.
6. **Submit and Integrate the Domain:** Submit your domain to the Workflomics repository and update the database.

Now, let’s go through each of these steps in detail.

Step 1: Clone the Workflomics Repository
========================================

To start, fork the `workflomics/tools-and-domains <https://github.com/Workflomics/tools-and-domains>`_ repository on GitHub (or create a new branch) and clone the forked repository to your local machine:

.. code-block:: bash

   git clone https://github.com/Workflomics/tools-and-domains.git

Navigate to the cloned directory:

.. code-block:: bash

   cd tools-and-domains

This repository contains templates and configuration files that will serve as a starting point for creating your new domain. In addition, the latest EDAM version (used to describe domain terminology, data types, formats, operations) and all the tools and domains hosted on the Workflomics platform are stored in this repository.

Step 2: Create a New Domain Folder
==================================

Create a new domain folder within the `domains` directory. This folder will store all configurations and tool annotations for your domain. You can copy the structure of the `template-domain` folder:

.. code-block:: bash

   mkdir domains/my-domain
   cp -r domains/template-domain/* domains/my-domain/

Your domain folder should have the following structure (the specified file names are recommended but can be customized):

.. code-block::

   my-domain/
   ├── tools.json
   ├── config.json
   └── constraints.json

**Explanation of Files:**

- **`config.json`**: Defines domain-specific metadata and configuration. This includes paths, input and output types, and tool references.
- **`tools.json`**: Contains bio.tools annotations for tools in your domain. You will populate this file with the list of tools used in the domain.
- **`constraints.json`**: Specifies constraints for tools and workflows in your domain. These constraints can be included or linked in `config.json`.

Step 3: Add Tools to Your Domain
================================

This section explains how to add tools to your domain, i.e., how to update the `tools.json` file with the bio.tools annotations and references to CWL descriptions for each tool (these CWL descriptions specify how the tool should be executed are are stored in the `tools-and-domains/cwl-tools <https://github.com/Workflomics/tools-and-domains/tree/main/cwl-tools>`_ directory). 
To add tools to your domain, follow the process below to either reuse existing annotations from the `cwl-tools` repository or create new annotations if the tool is not yet included.

1. **Check for Existing Tools in `cwl-tools`:**

   - Begin by searching for the tool on `bio.tools <https://bio.tools/>`_ and note down its `biotoolsID`. For example, the `biotoolsID` for the tool `Comet <https://bio.tools/comet>`_ is `comet`, while for `CoMet <https://bio.tools/comet-universe>`__ it is `comet-universe`.
  
   .. note::
      **How to obtain `biotoolsIDs`**

      The `biotoolsID` for each tool can be obtained from bio.tools. For example, the `biotoolsID` for the tool `comet <https://bio.tools/comet>`_ is `comet`. It is visible in the URL of the tool page. Alternatively, you can use bio.tools REST API to fetch the `biotoolsID` for a tool, see `comet entry <https://bio.tools/api/tool/comet>`_.

   - Use the `biotoolsID`` to check if the tool is already annotated in the `cwl-tools <https://github.com/Workflomics/tools-and-domains/tree/main/cwl-tools>`_ directory, where each tool is stored in a folder named after its `biotoolsID`. For example, the `Comet`` tool is annotated in the `cwl-tools/comet <https://github.com/Workflomics/tools-and-domains/tree/main/cwl-tools/comet>`_ directory.
   
   If the tool exists, simply copy the content from `cwl-tools/biotoolsID/biotoolsID.json` and paste it into the `your-domain/tools.json` file under your domain directory. This way, you can add tools without needing to modify or create any new CWL descriptions.

   For example, here is the full annotation for the `Comet` tool:

   .. code-block:: json

      {
        "outputs": [
          {
            "format_1915": ["http://edamontology.org/format_3655"],
            "data_0006": ["http://edamontology.org/data_0945"]
          },
          {
            "format_1915": ["http://edamontology.org/format_3247"],
            "data_0006": ["http://edamontology.org/data_0945"]
          },
          {
            "format_1915": ["http://edamontology.org/format_3475"],
            "data_0006": ["http://edamontology.org/data_0945"]
          }
        ],
        "inputs": [
          {
            "format_1915": [
              "http://edamontology.org/format_3244",
              "http://edamontology.org/format_3654",
              "http://edamontology.org/format_3651"
            ],
            "data_0006": ["http://edamontology.org/data_0943"]
          },
          {
            "format_1915": ["http://edamontology.org/format_1929"],
            "data_0006": ["http://edamontology.org/data_2976"]
          }
        ],
        "taxonomyOperations": ["http://edamontology.org/operation_3646"],
        "implementation": {
          "cwl_reference": "https://raw.githubusercontent.com/Workflomics/tools-and-domains/refs/heads/main/cwl-tools/comet/comet.cwl"
        },
        "biotoolsID": "comet",
        "label": "Comet",
        "id": "Comet"
      }

   Double-check that the `cwl_reference` field is correct and points to the appropriate CWL file in the repository. The `cwl_reference` should be accessible and point to the raw file URL of the CWL description for this tool in the `cwl-tools` directory.

2. **Adding New Tools from `bio.tools` Not Present in `cwl-tools`:**

   If the tool is not already annotated in the `cwl-tools` repository, follow the instructions in the :doc:`adding-tools` page, which explains how to create new CWL files and annotations for the tool.

   Once you have added the new tool to `cwl-tools`, made a PR and merged the changes into the `main` branch, update your domain's `tools.json` file using the same process as above, linking to the new CWL file using the `cwl_reference` field.

For additional guidance on how to create new CWL files and annotations, we refer to the `TESS CWL user guide <https://tess.elixir-europe.org/materials/cwl-user-guide>`_.


Step 4: Configure Domain Files
==============================

Edit `config.json`
^^^^^^^^^^^^^^^^^^

The `config.json` file contains most of the bioinformatics domain-specific metadata and configuration (e.g., path to the latest EDAM ontology, EDAM identifiers for root terminology - data format, data type, operation, etc.). You should update the `config.json` file  with your domain's specific configurations:

- Update paths for `tool_annotations_path` and `constraints_path` to point to the correct files in your domain folder (paths can be local while you are testing the domain, but when making a PR the paths should point to the expected "raw" path on `main`, as used in the template).
- Define `inputs` and `outputs` for the domain to reflect a demo example of the expected inputs and outputs for the tools in your domain. The terminology used adheres to EDAM classes and URIs (always use the latest EDAM version). As an example, the `config.json` provided in the template folder should contains `input` fields in the following format:

.. code-block:: json

   {
      "inputs": [
      {
         "data_0006": ["data_0943"],
         "format_1915": ["format_3244"]
      },
      {
         "data_0006": ["data_2976"],
         "format_1915": ["format_1929", "format_3654"]
      }],
   }

This specifies that the workflow will accept two distinct inputs. The first one must be of data type (`data_0006`) - `Mass spectrum` (`data_0943`) and data format (`format_1915`) - `mzML` (`format_3244`). The second input must have data type (`data_0006`) - `Protein sequence` (`data_2976`), while data format (`format_1915`) specifies two possible allowed formats `FASTA` (`format_1929`) and `XML` (`format_3654`). The output fields should be defined in a similar manner following the same semantics, the only difference is that the `inputs` field should be replaced with `outputs`.

For a full list of configurable options, see the `configuration documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/domain.html#core-configuration>`_.

Edit `tools.json`
^^^^^^^^^^^^^^^^^

The `tools.json` file holds the bio.tools annotations for all tools in your domain. At this stage, you should have updated this file with the correct tool annotations and CWL references for each tool. If you however want to generate the domain from scratch (and not use the existing CWL files and provided json annotations), you can the APE CLI to generate the `tools.json` file from a list of bio.tools IDs.:

.. code-block:: bash

   java -jar APE-2.4.0-executable.jar convert-tools ./toolIDsList.json

Refer to the `APE CLI documentation <https://ape-framework.readthedocs.io/en/v2.4/docs/developers/cli.html#convert-tools>`_ for more details on generating tool annotations.


Edit `constraints.json`
^^^^^^^^^^^^^^^^^^^^^^^

Modify the `constraints.json` file to include domain-specific constraints such as tool dependencies, data types, and workflow limitations. This file can be referenced in `config.json`, as currently done in the template, or included directly in the `config.json` file under the `constraints` field.

For more details on constraint formatting, see the `constraints documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/constraints.html#constraint-templates>`_.



Step 5: Validate the Domain with APE Library
============================================

After configuring the domain, validate the domain files using the APE library to check for errors:

.. code-block:: bash

   java -jar APE-2.4.0-executable.jar synthesis ./domains/my-domain/config.json

This command will validate your `config.json` and related files, ensuring that all inputs, outputs, and constraints are correctly defined. In addition, the command will generate workflows that fit the configuration specified (inputs, outputs, constraints) and check for any errors or inconsistencies. Make sure that this configuration produces at least one valid workflow, as it will be used as a demo example for the domain on the Workflomics platform.

Step 6: Submit and Integrate the Domain
=======================================

If the validation is successful, create a pull request to merge your changes into the Workflomics repository. The pull request should be reviewed and approved by the Workflomics development team.

Once the pull request is merged:

1. Create an issue in the `Workflomics repository <https://github.com/Workflomics/workflomics-frontend/issues/new/choose>`_ to request the addition of your domain to the database.
2. Include the domain name, a brief description, and the link to your domain's `config.json` file.
3. Update the database using the `SQL script <https://github.com/Workflomics/workflomics-frontend/blob/main/database/03_import_data.sql>`_ that contains the new domain information.

The Workflomics development team will finalize the integration and update the Workflomics platform to include your domain.

Configure CWL Files
===================

CWL files for the tools in your domain should be added to the `cwl-tools` directory and annotated according to bio.tools standards. Ensure each tool has a separate CWL file named after the tool, such as `Comet.cwl`, `PeptideProphet.cwl`, etc.

Once the CWL files are added, update `tools.json` to include the correct `cwl_reference` links.

For more information on creating and formatting CWL files, refer to the Elixir `Training Platform <https://tess.elixir-europe.org/materials/cwl-user-guide>`_.

Configure Workflomics
=====================

To integrate a new domain into the Workflomics platform, ensure the domain configuration is included in the `public.domain` table of the Postgres database. This can be done using the SQL script provided in the repository:

.. code-block:: sql

   INSERT INTO public.domain (name, description, config_path) VALUES ('my-domain', 'A new bioinformatics domain', 'domains/my-domain/config.json');

After updating the database, restart the Workflomics server to reflect the new domain changes.

If you have any questions or need assistance, please contact the `Workflomics development team <https://workflomics.readthedocs.io/en/domain-creation/#contributors>`_.
