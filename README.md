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
## Running an Experimental Reconstruction on Dragonfly

### Reformatting h5 file for conversion
The h5 file has the following format beforehand: 
```
/              Group
/intensities   Dataset {500000, 1, 128, 128}
...
/photons       Dataset {500000, 1, 128, 128}
...
```

The h5 file needs to be reformatted: 
```
python
>>> import h5py
>>> import numpy as np
>>> file = h5py.File('h5_file_name.h5', 'r')
>>> new_file = h5py.File('new_h5_file.h5')
>>> # Will only be putting the reformatted intensities and photons datasets into the new h5 file
>>> # However, the other ones can be added through the same process
>>> # Make sure the datasets are added to the new file in the correct order
>>> intensities = file['intensities']
>>> new_file['intensities'] = np.squeeze(intensities, axis=1)
>>> photons = file['photons']
>>> new_file['photons'] = np.squeeze(photons, axis=1)
>>> new_file.close()
>>> exit()
```

### Making a Reconstruction Directory
Now the h5 file can be converted into emc for experimental reconstruction on Dragonfly. You need to have already made a reconstruction directory spi_000x for the reconstruction. 
```
dragonfly_init -t spi
```

### Change [parameters] in config.ini to match data
The parameters in config.ini need to be changed to match the data. 
Use `nano config.ini` or another text editor to make changes to config.ini 

### Creating the Detector File
We have to make the detector file. 
A mask file can be used if available. To do so, add an 'in_mask_file' line with the file path of the custom mask file. Otherwise use the default configuration shown below. 
```
[make_detector]
out_detector_file = data/det_sim.h5
```

Then run the following command to create the detector file. 
```
./utils/make_detector.py
```

### Data Conversion from h5 to emc
```
cd spi_0003
./utils/convert/h5toemc.py -d photons new_h5_file.h5
```

### Updating EMC file locations in config.ini
Create a txt file with the locations of the emc file(s). 
```
cat > in_photon_list.txt
data/new_h5_file.emc
```

Update the in_photons_list in config.ini using a text editor. 
```
[emc]
in_photons_list = in_photon_list.txt
in_detector_file = make_detector:::out_detector_file
output_folder = data/
log_file = EMC.log
num_div = 10
need_scaling = 1
beta = 0.001
beta_schedule = 1.41421356 10
```

### Running the Reconstruction
The command below will run 100 iterations. This can be changed to your liking. 
```
./emc 100
```
