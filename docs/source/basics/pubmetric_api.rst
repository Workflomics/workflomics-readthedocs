Pubmetric RESTful API Methods
=============================


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
  - **Content:** application/ json containing metric and age benchmarks. #should this be application?


Recreate Graph
--------------

**Endpoint:** ``POST /recreate_graph``

Regenerates the co-citation graph on demand. 

- **Parameters:** 

  - ``topic_id`` (string|None): The topic ID from which tools are downloaded, or None for full bio.tools.
  - ``tool_list`` (string|None): A list of tools which will be used to generate the graph. 
  - ``tool_selection`` (string|None): Premade lists of annotated tools, "full" for all well annotated tools, "workflomics" for tools present in workflomics.

- **Success Response:**

  - **Code:** 200 
  - **Content:** "Graph and metadata file recreated successfully"  


Periodic Graph Generation
-------------------------
Automatically regenerates the co-citation graph on a monthly basis. 

- **Success Response:**

  - **Code:** 200 
  - **Content:** "Graph and metadata file recreated successfully"  


