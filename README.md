# Tree Ten
A generic pathogen clustering workflow based upon [Tree Nine](https://github.com/aofarrel/tree_nine).

### License
Software contained herein is LGPL. The provided sample data under the folder "example_data/" can be considered public domain, but keep in mind most of that is fake or heavily modified for easy testing, so maybe don't include it in real-world analysis.

## Quickstart
1. Make sure Docker is running
2. `pip install miniwdl` if you don't already have it
3. `miniwdl run simple_cluster.wdl --input example_jsons/simple_cluster.json --copy-input-files --verbose`

Cromwell is also supported. Non-Docker setups (Singularity, etc) *might* work but are not supported. See /documentation for detailed instructions.
