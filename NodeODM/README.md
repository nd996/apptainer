# NodeODM

Build the container
- Clone the repo somewhere e.g.`~/NodeODM`
- Edit the apptainer.def file to remove the `#. /var/www` line
- build the container (on the `admin01` node)
- Run the container, bind the repo and have it run the `npm install` command:
```bash
apptainer exec --bind NodeODM:/var/www nodeODM.sif npm install --production /var/www
```
- Run the container with the binded repo, it should now run and be able to write the folder
```bash
apptainer run --bind NodeODM:/var/www nodeODM.sif
```
- Port forward (needs to handle the compute node)
```
ssh -L 6367:localhost:6367 vik1
```

## Notes:
The container needs a copy of the repo to be writeable to the user. The original instructions wanted a writeable container but this needs `root` to be able to create it **and** to run it. Not good for the HPC. This method should hopefully work, unless more directories need to be writeable.

On first run we need to install the `npm` environment in the NodeODM folder, so on first run only load the container with:

```bash
apptainer exec --bind NodeODM:/var/www nodeODM.sif npm install --production /var/www
```

## Quickstart

```bash
apptainer exec --bind NodeODM:/var/www nodeODM.sif npm install --production /var/www
apptainer run --bind NodeODM:/var/www nodeODM.sif
```

