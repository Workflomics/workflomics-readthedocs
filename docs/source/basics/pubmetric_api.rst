Pubmetric RESTful API
=====================

Introduction ... . Once the RESTful API is up and running it automatically regenerates the co-citation graph on a monthly basis. 

Setup
-----




Index
-----

Score workflows
---------------

**Endpoint:** ``POST /score_workflow``

Calculates the `workflow_average` and the `tool_average` metric using the latest co-citation graph, and an uploaded CWL file.
Generates benchmarks for the metric scores as well as the ages of tools. 

- **Parameters:** 

  - ``cwl_file`` (File): A CWL file containing the workflow

- **Success Response:**

  - **Code:** 200 
  - **Content:** application/ json containing metric and age benchmarks. 


Recreate Graph
--------------

**Endpoint:** ``POST /recreate_graph``

Regenerates the co-citation graph on demand. 

- **Parameters:** 

  - ``topic_id`` (string|None): The topic ID from which tools are downloaded, or None for full bio.tools.
  - ``tool_selection`` (string|None): Premade lists of annotated tools, "full" for all well annotated tools, "workflomics" for tools present in workflomics. Or, a user specified list of tools (bio.tools tool names) which will be used to generate the graph. 
  - ``inpath`` (string|None): The directory from which an old metadata file should be loaded. Usually not used, as it does not download potential new additions to bio.tools.


- **Success Response:**

  - **Code:** 200 
  - **Content:** "Graph and metadata file recreated successfully!"  



