# MXNET R Package binaries for Windows

> Last update from the 20180507 build. Version 1.2.0. Build on R 3.5.0.


### CPU

```
install.packages("https://s3.ca-central-1.amazonaws.com/jeremiedb/share/mxnet/CPU/mxnet.zip", repos = NULL)
```

### GPU

```
install.packages("https://s3.ca-central-1.amazonaws.com/jeremiedb/share/mxnet/GPU/mxnet.zip", repos = NULL)
```


### Procedure to build R-package from pre-build Windows library

Download latest build from: https://github.com/yajiedesign/mxnet/releases

Copy the mxnet dll in the folder `R-package/inst/libs/x64/`:     

- build/libmxnet.dll

Dependency libraries should also be present in `R-package/inst/libs/x64/` and taken from the `vc14 base package` from the daily releases:  

- opencv_ffmpeg320_64.dll  
- libopenblas.dll  
- vcomp140.dll  
- libquadmath-0.dll  
- libgfortran-3.dll  
- libgcc_s_seh-1.dll  
- unzip32.dll

Copy the following folders in `R-package/inst/include/`:  

- include/mxnet  
- include/dlmc  
- include/mshadow  
- nnvm/include/nnvm

Initial NAMESPACE in R-Package:  

```
import(Rcpp)  
import(methods)
```

### Temporary patch

`im2rec` currently results in crashes during the build. 
Remove the `im2rec.h` and `im2rec.cc` files in `src/` and comment out the two im2rec lines in `/src/mxnet.cc`. 

#### Building the R-package

In bash with admin rights: 

```
Rscript -e "if(!require(devtools)){install.packages('devtools', repo = 'https://cloud.r-project.org/')}"
Rscript -e "library(devtools); library(methods); options(repos=c(CRAN='https://cloud.r-project.org/')); install_deps(pkg='R-package', dependencies = TRUE)"
R CMD INSTALL R-package
Rscript -e "require(mxnet); mxnet:::mxnet.export('R-package')"
rm -r R-package/NAMESPACE
Rscript -e "if (!require('roxygen2')||packageVersion('roxygen2')!= '5.0.1'){devtools::install_version('roxygen2',version='5.0.1',repo='https://cloud.r-project.org/',quiet=TRUE)}"
Rscript -e "require(roxygen2); roxygen2::roxygenise('R-package')"
R CMD INSTALL R-package
```
