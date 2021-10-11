## Running a Dragonfly Experimental Reconstruction on Cori

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
The command below will run 100 iterations. This can be changed to your liking by changing the number after -i 
```
salloc --nodes 4 --qos interactive --time 03:00:00 --constraint haswell
srun --nodes 4 -c 64 ./utils/run_emc.py -i 100
```
