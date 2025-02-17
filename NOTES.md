
# Ubuntu stop unattended upgrades
```
sudo dpkg-reconfigure -plow unattended-upgrades
```

# Fix keyboard layout
```
sudo dpkg-reconfigure keyboard-configuration
```


### dont use, trynewer GCC
> apt install g++-4.8 gcc-4.8 
> sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 10
> sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 10


apt install libbz2-dev libcurl4-openssl-dev

./configure --disable-xrootd  --disable-krb5 --disable-odbc --disable-oracle --disable-pgsql --disable-qt  --enable-unuran --enable-table --enable-explicitlink --enable-minuit2 --enable-roofit --enable-cxx14 --disable-davix


### fix the gfal define

```
sed -i 's/#ifdef _GFAL2_API_/#ifdef _GFAL2_API_ || defined(GFAL2_API_H_)/' /opt/root/io/gfal/src/TGFALFile.cxx
```
make


## Use cmake instead


```diff
--- /opt/root/tmva/src/RuleFitParams.cxx.orig	2025-02-14 14:47:05.706328347 +0000
+++ /opt/root/tmva/src/RuleFitParams.cxx	2025-02-14 14:48:05.158148747 +0000
@@ -24,6 +24,7 @@
  * (http://tmva.sourceforge.net/LICENSE)                                          *
  **********************************************************************************/
 
+#include <cmath>
 #include <iostream>
 #include <iomanip>
 #include <numeric>
@@ -880,7 +881,7 @@
       fstarVal = fRuleEnsemble->FStar(e);
       fFstar.push_back(fstarVal);
       fstarSorted.push_back(fstarVal);
-      if (isnan(fstarVal)) Log() << kFATAL << "F* is NAN!" << Endl;
+      if (std::isnan(fstarVal)) Log() << kFATAL << "F* is NAN!" << Endl;
    }
    // sort F* and find median
    std::sort( fstarSorted.begin(), fstarSorted.end() );
```


### commands to run
``` bash
wget
tar
cd root-version
# cmake may take care of this
#sed -i 's/#ifdef _GFAL2_API_/#ifdef _GFAL2_API_ || defined(GFAL2_API_H_)/' /opt/root/io/gfal/src/TGFALFile.cxx

# fix broken mirror
sed  -i 's|http://mirror.switch.ch/ftp/mirror/gnu/gsl|https://ftp.gnu.org/gnu/gsl|g' ../cmake/modules/SearchInstalledSoftware.cmake
# add patch (add thw above to patch too?
# patch < fix-cmath-include.patch

#mkdir root_build && cd root_build
cd build
cmake -DCMAKE_CXX_FLAGS="-D_GFAL2_API_ -std=c++11 -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include" -Dbuiltin_gsl=ON -DCMAKE_INSTALL_PREFIX=/usr  ..
#cmake --build . -- -j$(nproc --all)
cmake --build . -- install -j$(nproc --all)

### build acqu
cd /opt
git clone https://github.com/A2-Collaboration/acqu.git
cd ./acqu/
# last commit, in case things change
git reset --hard b7571696ec72e2b08d64d7640f9ed5a39eddb9f6
mkdir build && cd build && cmake ..
make -j$(nproc all)

```




### Notes
Needs 
- `std=c++11`
- `builtin_gsl=ON` Ubuntu installed version is too new
- Include dirs `-I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include"`






### Ignore
/usr/include/gfal2/common/gfal_common.h
```
#include "glib-2.0/glib.h"
```
