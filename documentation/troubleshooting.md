# Troubleshooting

## Crashes immediately
### Something about not finding Docker daemon/Docker socket
miniwdl/Cromwell cannot boot on its own. Turn on Docker Desktop (Mac, Windows) or make sure Docker Engine is running (Linux) then try again.


## Clustering
### Polars throws a warning about CPU features then crashes
Your hardware is not compatiable with polars. Most likely, this is caused by running on an ARM backend.

### Crashes in process_clusters.py and complains about not finding a "rosetta stone" file (not to be confused with "Rosetta" in the x86/ARM sense)
You do not have any clusters at a particular SNP distance. Create a bogus maple diff that is an exact or off-by-one-SNP copy of another sample, give it a unique name, add it to the usher tree, then it should work. If you are running with cluster_entire_tree = false, make sure to include your bogus sample in the samples_considered_for_clustering file. This is an edge case I hope to handle more gracefully in the future.
