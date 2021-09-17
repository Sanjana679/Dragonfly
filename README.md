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
git clone https://github.com/irischang020/Dragonfly.git
cd Dragonfly
mkdir build
cd build
module load modules/3.2.11.4
module load intel/19.0.3.199
module load altd/2.0 module load darshan/3.2.1
module load craype/2.6.2 
module load craype-network-aries
module load cray-libsci/19.06.1
module load craype-haswell
module load ugni/6.0.14.0-7.0.1.1_7.59__ge78e5b0.ari         
module load cray-mpich/7.7.10
module load pmi/5.0.14
module load cmake/3.20.5
module load dmapp/7.1.1-7.0.1.1_4.68__g38cf134.ari           
module load gsl/2.5
module load gni-headers/5.0.12.0-7.0.1.1_6.44__g3b1768f.ari  
module load cray-hdf5/1.10.5.2
module load xpmem/2.2.20-7.0.1.1_4.26__g0475745.ari
module load python/3.8-anaconda-2020.11
module load cmake/3.20.5
module load gsl
module load cray-hdf5
module load job/2.2.4-7.0.1.1_3.53__g36b56f4.ari
module load dvs/2.12_2.2.167-7.0.1.1_17.9__ge473d3a2
module load alps/6.6.58-7.0.1.1_6.26__g437d88db.ari
module load rca/2.2.20-7.0.1.1_4.70__g8e3fb5b.ari
module load atp/2.1.3
module load PrgEnv-intel/6.0.5
module load craype-hugepages2M
module load atp/2.1.3
module load cray-libsci/19.06.1 
cmake ..
make
make install
export PATH=/global/homes/i/ihchang/.local/dragonfly/bin:$PATH #add to .bashrc file
#export PATH=/global/homes/n/nand1234/.local/dragonfly/bin:$PATH #add to .bashrc file
cd ..
dragonfly_init
```
