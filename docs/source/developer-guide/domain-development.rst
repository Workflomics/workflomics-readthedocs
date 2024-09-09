Domain Development
##################

This guide outlines the steps to add a domain to the Workflomics platform, and how to maintain the existing domains.

Create New Domain
*****************

Setup the Domain Description
============================

1. Fork the `workflomics/domains annotations <https://github.com/Workflomics/domain-annotations>`_ repository on GitHub and add a new domain folder, e.g., `my-domain`, to the repository.
2. Create the directory structure for the domain (see `template <https://github.com/Workflomics/domain-annotations/tree/main/template>`_ for reference). The domain directory should contain the following files:

   .. code-block:: text

      my-domain/
      ├── bio.tools.json
      ├── config.json
      └── constraints.json

3. Fetch bio.tools annotations using the [TODO: add instructions] interface of RESTful APE. The annotations can be modified [TODO: We need to decide what to do in that case?]
4. The tool annotations should point to CWL files (uploaded to `cwl tools <https://github.com/Workflomics/containers/tree/main/cwl/tools>`_).

5. Adjust the default APE configuration in the `config.json` file. The `template` should provide most of the fields already defined, such as `ontology_path`, . For more information on the configuration options, see the `APE documentation <https://ape-framework.readthedocs.io/en/latest/docs/specifications/setup.html#core-configuration>`_.
   
6. Domain constraints - this is useful to explain (with examples if possible)

7. Define demo input types - we should decide how to handle benchmarks

8. Define domain benchmarks - we should provide a standardised way to describe benchmarks.

9. Test the domain configuration by running the APE server with the new domain.
   

Configure Workflomics
=====================

The new domain should be added to the Postgres database on the Workflomics platform. 



Update Existing Domain
**********************

This section describes how to update an existing domain on the Workflomics platform. 

Update Domain Annotations
=========================

To update the domain annotations you can u


