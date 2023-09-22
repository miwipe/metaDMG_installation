# metaDMG_installation
This installs a previous version of metaDMG that are compatible with the GUI dashboard. A new and updated version will soon be released.

# Installing metaDMG using conda and pip

### Create an conda environment and activate it
```
mamba create -n metaDMG python=3.9.15 htslib=1.17 eigen=3.4.0 cxx-compiler=1.5.2 c-compiler=1.5.2 gsl=2.7
conda activate metaDMG
```

### htslib is important for reading sam/bam file formats
```
git clone https://github.com/SAMtools/htslib
cd htslib
git submodule update --init --recursive
make
```

### Install the metaDMG-cpp if the HTSSRC=systemwide doesn't work, replace systemwide with the path to the htslib folder
```
cd ../
git clone https://github.com/metaDMG-dev/metaDMG-cpp.git
cd metaDMG-cpp
make clean && make CPPFLAGS="-L${CONDA_PREFIX}/lib -I${CONDA_PREFIX}/include" HTSSRC=systemwide -j 8
```

### Installing the viz (visualizer/dashboard) 
```
pip install git+https://github.com/metaDMG-dev/metaDMG-core@stopiferrors_branch
pip install iminuit==2.17.0 numpyro==0.10.1 joblib==1.2.0 numba==0.56.2 flatbuffers==22.9.24 logger_tt==1.7.2 psutil==5.9.4
pip install metaDMG[viz]
```

# Running metaDMG

### Create a config file, and remember to modify the settings in it, e.g. damage mode lca or local, max_position 15/30, bayesian or not etc.
```
metaDMG config *.merged.sort.sam.gz --names /science/willerslev/edna/ncbi_taxonomy_01Oct2022/names.dmp --nodes /science/willerslev/edna/ncbi_taxonomy_01Oct2022/nodes.dmp --acc2tax /science/willerslev/edna/ncbi_taxonomy_01Oct2022/combined_accession2taxid_20221112.gz -m /willerslev/edna/programmes/metaDMG-cpp/metaDMG-cpp
```

### Once done with the editing of the config.yaml file then execute the calculations, suggestion perhaps try it for a small subset first
```
metaDMG compute config.yaml
```

### Once metaDMG is done, you can explore the data with our gui. execute this command where you execute the compute command.
```
metaDMG dashboard config.yaml
```

### Now open a new terminal and connect via SSH to your server, it can also be run locally, and if so you do not need to setup this connection
```
ssh -L 8050:127.0.0.1:8050 -N User@YourServer
```

### Now open your browser (I use Chrome) and paste this address
```
http://0.0.0.0:8050/
```

### Print the metaDMG output to a csv file
```
metaDMG convert --output MetaDMG_results.csv --add-fit-predictions
```

