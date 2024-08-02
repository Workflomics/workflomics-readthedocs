Domain Development
##################

This guide outlines the steps to add a domain to the Workflomics platform, and how to maintain the existing domains.

Create New Domain
*****************

Setup the Domain Description
============================

1. Fork the `workflomics/domains` repository on GitHub and add a new domain folder, e.g., `new domain`, to the repository.
2. Create the directory structure for the domain:

   .. code-block:: text

      new-domain/
      ├── domain-annotations/
      ├── workflows/
      └── README.md

3. Fetch bio.tools annotations using the [TODO: add instructions] interface of RESTful APE. The annotations can be modified [TODO: We need to decide what to do in that case?]

4. Setup the default `APE Config`
   
5. Domain constraints - this is useful to explain (with examples if possible)

6. Define demo input types - we should decide how to handle benchmarks

7. Define domain benchmarks - we should provide a standardised way to describe benchmarks.
   

Configure Workflomics
=====================

The new domain should be added to the Postgres database on the Workflomics platform. 



Update Existing Domain
**********************

This section describes how to update an existing domain on the Workflomics platform. 

Update Domain Annotations
=========================

To update the domain annotations you can u


