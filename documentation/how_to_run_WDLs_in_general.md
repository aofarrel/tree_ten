# How To Run WDLs in General

> [!NOTE]  
> This guide is designed for people who are new to WDL, but is only as a bare bones "get most people up and running ASAP" introduction. If you run into problems here, consider going through one of the various WDL tutorials online first, then coming back here.

Tree Ten is provided as WDL workflow. WDL workflows are comparable to CWL, Cloud Run, or SLURM workflows that you submit, rather than a software library.

## Running on Terra
Tree Ten supports running on [Terra](https://terra.bio/) (formerly Firecloud), a bioinformatics platform that leverages Google Cloud to run WDLs with relative ease. You will need to set up the appropriate billing project with Google.

Once you have your Terra account set up, import the Dockstore version of this workflow in a new Terra workspace. It will appear in the "Workflows" tab, from which you can run it.

Actually entering data into Terra can be a little tricky if you're not used to it. As it operates in the cloud, your data must also be in the cloud; this can be achieved by uploading it to the Terra workspace's Google bucket (see "Data"). You can then copy-pasting the resulting gs:// URIs into your workflow inputs, but you'll probably instead want to use [data tables](https://support.terra.bio/hc/en-us/articles/360025758392-Managing-data-with-tables) if you're doing anything beyond a small test run. If using data tables, it's recommend to create one such that each row represents one sample.

## Running on anything else
You will need:
* Docker
  * Podman, rootless Docker, or Singularity *might* work but are not officially supported
* A WDL executor

We also recommend x86 hardware if possible, as Tree Ten contains three libraries (UShER, BTE, and a particular version of polars) that would prefer not to go through Rosetta's compatability layer. It should work, but expect decreased performance.

### Docker
Docker is available for Windows, Mac OS, and basically all flavors of Linux. If you are using Linux, it is recommended to install [Docker Engine](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) instead of Docker Desktop due to differences in virtualization. For Windows and Mac OS, Docker Desktop should be fine.

#### What if my institute's HPC doesn't allow me to use Docker for security reasons?
You have multiple options:   
a) Consult [Dockstore's documentation on Docker alternatives](https://docs.dockstore.org/en/stable/advanced-topics/docker-alternatives.html)  
b) Tree Ten is quite scalable; you might be able to get away with running it locally if you have only a few (<200ish) samples   
c) Arm-wrestle your sysadmin   

Although we cannot support non-Docker versions of this workflow, we happily accept documentation contributions from people who have managed to get Tree Ten running on non-Docker setups.

### WDL Executor
Unlike most languages, WDL does not have a canonical implementation. Here are the most common executors:
* [miniwdl](https://github.com/chanzuckerberg/miniwdl) (`pip install miniwdl`)
* [Cromwell](https://cromwell.readthedocs.io/en/latest/tutorials/FiveMinuteIntro/) (JAR file; requires Java)
* [the Dockstore CLI](https://dockstore.org/quick-start) (wrapper for Cromwell)
* [TOIL](https://toil.ucsc-cgl.org/) ([WDL support is in beta](https://toil.readthedocs.io/en/latest/wdl/running.html) and not tested with these workflows)

Although Cromwell is what is used on Terra and fully supported, it should be noted Cromwell does not handle system resources properly if it is not running on the cloud. Specifically, Cromwell will occasionally cause the underlying Docker runtime to lock up, requiring a restart. This is very unlikely to happen when running Tree Nine due to its very limited use of concurrent tasks, but if you are seeing strange behavior with Cromwell, try [setting concurrent-job-limit in a Cromwell configuration file as guided here](https://docs.dockstore.org/en/stable/advanced-topics/dockstore-cli/local-cromwell-config.html).

# How To Actually Run It
* miniwdl: `miniwdl run [workflow_name.wdl] -i [inputs.json] --copy-input-files`
* Cromwell: `java -jar cromwell.jar run [workflow_name.wdl] -i [inputs.json]`
* Dockstore CLI: `dockstore workflow launch --entry [entry_name] --json [inputs.json]`
  * Unlike other WDL runners, you can run a workflow with the Dockstore CLI without making a local copy first, if that workflow is on Dockstore. In this situation, make sure to use workflow's entry name given on Dockstore, ex: github.com/aofarrel/myco/myco_sra:7.0.3
