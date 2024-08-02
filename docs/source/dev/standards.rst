###################################
Data Formats and Schemas Overview
###################################

This page provides an overview of the various data formats and schemas used within our project. 

**************************
JSON Schema for Benchmarks
**************************

The json structure is used to capture detailed benchmark results, which include both design-time and run-time metrics. Each benchmark is detailed at the tool level and aggregated at the workflow level. 


Design-Time Benchmarks
-----------------------
Design-time benchmarks include metadata such as:
- Supported Operating Systems (OS)
- License types
- Citation counts

Run-Time Benchmarks
-------------------
Run-time benchmarks include execution details such as:
- Execution status
- Execution time
- Memory usage
- Number of warnings and errors
- Number of identified proteins and GO-terms

JSON Schemas
------------

### Benchmark Schema

.. code-block:: json

    [
        {
            "workflowName": "string",
            "executor": "string",
            "runID": "string",
            "inputs": [
                {   
                    "cwl_id": "string",
                    "filename": "string",
                    "size": "number"
                }
    ],
            "benchmarks": [
                {
                    "description": "string",
                    "title": "string",
                    "unit": "string",
                    "aggregate_value": {
                        "value": "string|number",
                        "desirability": "number"
                    },
                    "steps": [
                        {
                            "label": "string",
                            "value": "string|number",
                            "desirability": "number",
                            "tooltip": ["string"]
                        }
                    ]
                }
            ]
        }
    ]


.. .. image:: images/run_time_example.png
..    :alt: Run-Time Benchmark Visualization

**Field Descriptions:**


+-----+-----+---------------------+----------+-----------------------------------------------+
|             Field               | Required | Description                                   |
+=====+=====+=====================+==========+===============================================+
|             ``workflowName``    | Yes      | The name of the workflow.                     |
+-----+-----+---------------------+----------+-----------------------------------------------+
|             ``executor``        | No       | The executor used for the run.                |
+-----+-----+---------------------+----------+-----------------------------------------------+
|             ``runID``           | No       | A unique identifier for the run.              |
+-----+-----+---------------------+----------+-----------------------------------------------+
|             ``inputs``          | No       | An array of input objects.                    |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``cwl_id``          | No       | The CWL identifier for the input.             |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``filename``        | No       | The name of the input file.                   |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``size``            | No       | The size of the input file.                   |
+-----+-----+---------------------+----------+-----------------------------------------------+
|             ``benchmarks``      | Yes      | An array of benchmark objects.                |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``description``     | Yes      | A description of the benchmark.               |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``title``           | Yes      | The title of the benchmark.                   |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``unit``            | Yes      | The unit of the benchmark.                    |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``aggregate_value`` | Yes      | Aggregated value for the benchmark.           |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |     | ``value``           | Yes      | The aggregated value.                         |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |     | ``desirability``    | Yes      | Aggregated desirability score.                |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |       ``steps``           | Yes      | An array of step objects detailing each tool. |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |     | ``label``           | Yes      | The name of the tool.                         |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |     | ``value``           | Yes      | The value for the benchmark step.             |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |     | ``desirability``    | Yes      | A score indicating desirability.              |
+-----+-----+---------------------+----------+-----------------------------------------------+
|     |     | ``tool``            | No       | Additional details for the step.              |
+-----+-----+---------------------+----------+-----------------------------------------------+


Note that run-time benchmark schema extends the design-time schema with additional fields for execution details.


Other Formats
-------------
In addition to the JSON formats described above, we use other data formats such as APE-specific domain annotations within the project. These formats are either described externally and referenced or will be added to this document in the future.

