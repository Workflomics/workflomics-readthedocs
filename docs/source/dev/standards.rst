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
            "inputs": {
                "input_1": {
                    "filename": "string"
                },
                "input_2": {
                    "filename": "string"
                }
            },
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


.. image:: images/run_time_example.png
   :alt: Run-Time Benchmark Visualization

**Field Descriptions:**
- `workflowName`: The name of the workflow.
- `executor`: The executor used for the run
- `runID`: A unique identifier for the run.
- `inputs`: The inputs for the workflow.
  - `input_1`: Details of the first input file.
    - `filename`: The name of the input file.
  - `input_2`: Details of the second input file.
    - `filename`: The name of the input file.
- `benchmarks`: An array of benchmark objects.
  - `description`: A description of the benchmark.
  - `title`: The title of the benchmark.
  - `unit`: The unit of the benchmark.
  - `aggregate_value`: Aggregated value for the benchmark.
    - `value`: The aggregated value.
    - `desirability`: Aggregated desirability score.
  - `steps`: An array of step objects detailing each tool.
    - `label`: The name of the tool.
    - `value`: The value for the benchmark step.
    - `desirability`: A score indicating desirability.
    - `tooltip`: Additional details for the step.


Note that run-time benchmark schema extends the design-time schema with additional fields for execution details.


Other Formats
-------------
In addition to the JSON formats described above, we use other data formats such as APE-specific domain annotations within the project. These formats are either described externally and referenced or will be added to this document in the future.

