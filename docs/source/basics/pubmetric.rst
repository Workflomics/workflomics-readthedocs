Pubmetric documentation
#######################

This guide provides instructions for setting up and using the `pubmetric` package, including generating graphs and calculating metrics.

Development Setup
*****************

**Prerequisites**:

- Ensure you have Python 3.7+ installed.
- Install the `pubmetric` package:

  .. code-block:: bash

     pip install pubmetric

**Importing the Package**:

1. Import the necessary modules in your Python script, pubmetric relies on asyncio and needs to be executed using it:

   .. code-block:: python 

      import pubmetric
      import asyncio

Graph Generation
*****************

**Generate New Co-citation Graph**:

2. **Test Size**: Create a smaller co-citation graph for testing purposes:

   .. code-block:: python

      testsize_graph = asyncio.run(pubmetric.create_network(test_size=20))
      print(testsize_graph.vs['age'])

3. **Full Network**:

   - Generate a full co-citation network for a specific topic:

     .. code-block:: python

        full_proteomics_graph = asyncio.run(pubmetric.create_network(topic_id="topic_0121", inpath="path/to/data"))
        full_metabolomics_graph = asyncio.run(pubmetric.create_network(topic_id="topic_3172"))

   - Generate a genomics co-citation graph, using not the topic, but all well-annotated tools currently in bio.tools by specifying topic_id=None and tool_selection='full':

     .. code-block:: python

        full_genomics_graph = asyncio.run(pubmetric.create_network(topic_id=None, inpath='path/to/data', tool_selection='full'))

Pubmetric outputs 3 files; doi_pmid_library.json, tool_metadata.json and graph.pkl. These can be used to load the graph, or recreate the graph using the already downloaded metadata.

**Load Data**:

4. Load an existing co-citation graph from a specified path:

   .. code-block:: python

      path_to_data = 'path/to/your/data'
      loaded_graph = asyncio.run(pubmetric.create_network(inpath=path_to_data, load_graph=True))

Metric Calculation
******************

**Download Workflow Data**:

5. Load workflow metadata and calculate the metric. CWL files can be downloaded for Worfklomics.org live demo. 
   The CWL parser function needs the tool_metadata.json which is generated alongside the ci-citation graph.
   There are two main metrics, ``workflow_average`` and ``complete_average`` which take into account only the workflow 
   edges, or all possible edges between tools in a workflow, respectively:

   .. code-block:: python

      cwl_file_path = "./path/to/candidate_workflow.cwl"
      metadata_file_path = 'path/to/tool_metadata.json'
      workflow = pubmetric.parse_cwl(cwl_file_path, metadata_file_path)
      metric_score = pubmetric.workflow_average(loaded_graph, workflow)

File Schemas
************

The package expects some specific schemas for certain files 


**The Metadata File:**

6. The metadata file holds the metadata also contained within the graph, with some additional global statistics on the data download. 
It can be used to regenerate a graph containing the tools in the file. By specifying inpath="./path/to/directory" containg the metadata file. 

.. code-block:: json

   {
      "creationDate": "<string>",
      "topic": "<string>",
      "totalNrTools": <integer>,
      "biotoolsWOpmid": <integer>,
      "pmidFromDoi": <integer>,
      "tools": [
         {
            "name": "<string>",
            "doi": "<string or null>",
            "topics": ["<string>", ...],
            "nrPublications": <integer>,
            "allPublications": ["<string>", ...],
            "pubDate": <integer>,
            "pmid": "<string>",
            "nrCitations": <integer>
         }
      ]
   }



The Workflow Dictionary
***********************

The workflow dictionary should follow the following schema:

.. code-block:: json

   {
       "edges": [
           [
               "<string>",
               "<string>"
           ],
           [
               "<string>",
               "<string>"
           ]
           ...
       ],
       "steps": {
           "<string>": "<string>",
           "<string>": "<string>"
           ...
       },
       "pmid_edges": [
           [
               "<string>",
               "<string>"
           ],
           [
               "<string>",
               "<string>"
           ]
           ...
       ]
   }

