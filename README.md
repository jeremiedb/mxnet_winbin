# MXNET R Package binaries for Windows

> Last update from the 20170514 build. Version 0.9.5

Package can be installed using the following command: 

```
install.packages("https://github.com/jeremiedb/mxnet_winbin/raw/master/mxnet.zip", repos = NULL)
```

###Procedure for building R-package from pre-build Windows library

Download latest build from: https://github.com/yajiedesign/mxnet/releases

Copy the mxnet dll in the folder `R-package/inst/libs/x64/`:     

- build/libmxnet.dll

Dependency libraries should also be present in `R-package/inst/libs/` and taken from the `vc14 base package` from the daily releases:  

- opencv_ffmpeg320_64.dll  
- libopenblas.dll  
- vcomp140.dll  
- libquadmath-0.dll  
- libgfortran-3.dll  
- libgcc_s_seh-1.dll  
- unzip32.dll

Then copy the following folders in `R-package/inst/include/`:  

- include/mxnet  
- include/dlmc  
- include/mshadow  
- nnvm/include/nnvm

Initial NAMESPACE in R-Package:  

```
import(Rcpp)  
import(methods)
```

Building the R-package

```
cd R-package
Rscript -e "library(devtools); library(methods); options(repos=c(CRAN='https://cran.rstudio.com')); install_deps(dependencies = TRUE)"
cd ..
R CMD INSTALL --no-multiarch R-package
Rscript -e "require(mxnet); mxnet:::mxnet.export(\"R-package\")"
rm -rf R-package/NAMESPACE
Rscript -e "require(roxygen2); roxygen2::roxygenise(\"R-package\")"
R CMD build --no-build-vignettes R-package
R CMD INSTALL --no-multiarch R-package
```
