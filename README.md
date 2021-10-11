# Dragonfly

*Dragonfly* is a software package to analyze single particle diffractive imaging data. The package has the following parts

* An implementation of the [EMC single-particle reconstruction algorithm](http://journals.aps.org/pre/abstract/10.1103/PhysRevE.80.026705) using MPI and OpenMP to merge diffraction patterns of approximately identical particles in random orientations.
* Data stream simulator, that generates noisy single-particle diffraction patterns from a [PDB](http://www.rcsb.org/pdb/home/home.do) file
* An experimental pattern-classification GUI using some machine learning tools to separate single particle diffraction patterns in experimental data.

More detailed documentation can be found in the [Wiki pages](https://github.com/duaneloh/Dragonfly/wiki).

If you are in a hurry, just clone the repository and follow the [Quick start](https://github.com/duaneloh/Dragonfly/wiki/Quick-start-with-simulations) instructions.

Please cite the following publication if you use *Dragonfly* for your work:
> Ayyer, K., Lan, T. Y., Elser, V., & Loh, N. D. (2016). Dragonfly: an implementation of the expand–maximize–compress algorithm for single-particle imaging. [*Journal of applied crystallography*, **49**(4), 1320-1335](https://doi.org/10.1107/S1600576716008165).

## Get started on psana
```
git clone https://github.com/irischang020/Dragonfly.git
cd Dragonfly
source /reg/g/psdm/etc/psconda.sh # python2 (py2 environment is "frozen" so cannot get updated; py2 has been discontinued)
source /reg/g/psdm/etc/psconda.sh -py3 # python3
mkdir build
cd build
cmake ../
make install
export PATH=/cds/home/i/iris/.local/dragonfly/bin:$PATH
mkdir ~iris/test_recons
cd test_recons
dragonfly_init
```

Sanjana's commands
```
git clone https://github.com/irischang020/Dragonfly.git
cd Dragonfly
source /reg/g/psdm/etc/psconda.sh 
mkdir build
cd build
cmake ..
make install
export PATH=/cds/home/n/nand1234/.local/dragonfly/bin:$PATH
cd ..
dragonfly_init
```

## Get started on Cori
```
git clone https://github.com/duaneloh/Dragonfly.git
cd Dragonfly
mkdir build
cd build
module load cmake/3.20.5
module load gsl
module load cray-hdf5
# Be sure to include the following modules as well
![image](https://user-images.githubusercontent.com/25139826/135107198-e9d64b18-f644-4aa3-9388-4423a6593238.png)
cmake ..
make
make install
export PATH=/global/homes/n/nand1234/.local/dragonfly/bin:$PATH #add to .bashrc file
cd ..
dragonfly_init
```
