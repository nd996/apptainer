# BioEmu Apptainer Definition File

Manually installs the Colabfold because by default when BioEmu is first run it will do this, but then run a job (which requires a GPU) - which we don't want to do when building a container. Patched in some Python code to run only the commands to setup the Colabfold.

