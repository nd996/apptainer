# Build & Run container

## Build

For debugging, don't clean up the build directory and log all the output to file
```bash
apptainer build --no-cleanup ROOT-acqu_Ubuntu18.sif ROOT-acqu_Ubuntu18_NEW.def 2>&1 | tee outfile
```

Production build
```bash
apptainer build ROOT-acqu_Ubuntu18.sif ROOT-acqu_Ubuntu18_NEW.def
```

## Running
Bind a local directory, not for this test you must be inside the local `acqu_user` dir which is a copy of from the `acqu` repo.
```bash
apptainer run --bind /home/nd996/src/Apptainer/ROOT/acqu_user:/opt/acqu_source/acqu_user ROOT-acqu.sif AcquRoot AR.dat
```
> NOTE: the `AR.dat` file describes `TreeFile: scratch/geant.root` which doesn't exist.




---

# Useful commands when running Ubuntu 18.04 in a VM

## Ubuntu stop unattended upgrades

By default Ubuntu with lock `apt` after first install, start upgrading the system and block you from installing anything.
```bash
sudo dpkg-reconfigure -plow unattended-upgrades
```

## Fix keyboard layout
```bash
sudo dpkg-reconfigure keyboard-configuration
```

## Install & set older default GCC
```bash
apt install g++-4.8 gcc-4.8 
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 10
```

## Use cmake instead of ./configure

### Build Notes
Needs:
- `std=c++11`
- `builtin_gsl=ON` Ubuntu installed version is too new
- Include dirs `-I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include"`


