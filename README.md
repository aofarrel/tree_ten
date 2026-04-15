# Tree Ten
A generic pathogen clustering workflow based upon [Tree Nine](https://github.com/aofarrel/tree_nine).

### License
Software contained herein is LGPL. The provided sample data under the folder "example_data/" can be considered public domain, but keep in mind most of that is fake or heavily modified for easy testing, so maybe don't include it in real-world analysis.

## Quickstart
1. Make sure Docker Engine is running (Mac OS: Docker Desktop needs to be running per the little whale in your top bar)
2. `pip install miniwdl` if you don't already have it
3. Make sure the test run works: `miniwdl run simple_cluster.wdl --input example_jsons/simple_cluster.json --copy-input-files --verbose`
4. Replace the following inputs in simple_cluster.json with your own data:
    * input_tree_pb: USHER .pb tree of samples
    * sample_metadata_tsv: Metadata TSV, use the sample as a template
    * diffs: Your maple diff files. **Unlike my Tree Nine workflow, it is assumed these diff files are samples already on the tree, but they also must be provided here too (needed for backmasking to work properly)**
5. Run it with your new JSON (same command)

Cromwell and Terra are also supported. Non-Docker setups (Singularity, etc) *might* work but are not supported. See /documentation for detailed instructions.
