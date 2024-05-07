Benchmark Workflows
===================

This documentation provides instructions on how to execute and benchmark workflows generated on the Workflomics platform, using the Workflomics Benchmarker CLI.


Running Workflows
-----------------

To run the Workflomics workflows:

1. Download workflows that were generated on the Workflomics website (using the "Download selected" button), and unzip them.

2. Navigate to the folder containing the unzipped workflows.

3. Execute the benchmark command in the terminal. To benchmark in the current directory:

   .. code-block:: bash

      workflomics benchmark .

   Or to benchmark in a different directory:

   .. code-block:: bash

      workflomics benchmark path-to-dir

.. note:: Replace `path-to-dir` with the actual path to the directory containing your Workflomics workflows.

Visualizing Benchmark Results
-----------------------------

After benchmarking, upload your results to the Workflomics platform for interactive visualization:

1. Navigate to the Workflomics Benchmark Upload Page:

   - `Workflomics Benchmarks Upload <http://145.38.190.48/benchmarks>`_

2. Upload your `workflow-benchmarks.json` file by following the on-screen instructions.

3. Access interactive visualizations to explore the benchmark results comprehensively.

.. note:: Please ensure that any data you upload is free of sensitive or proprietary information, as it will be accessible to other users for analysis and comparison purposes.
