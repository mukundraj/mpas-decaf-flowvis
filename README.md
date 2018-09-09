# mpas-decaf-flowvis

## Dependencies

Decaf  

- C++ 11  
- MPI-3  
- Boost 1.59 or higher  
- Networkx 2.0  
- CMake 3.0 or higher  

MPAS-O

- NetCDF  
- Parallel NetCDF  
- PIO  

Flowvis

- VTK (Tested with version 8.1)  
- Eigen  
- Libnabo (https://github.com/ethz-asl/libnabo)


# Steps to build and run test case


## Clone repository and submodules (MPAS and Decaf )
git clone https://github.com/mukundraj/mpas-decaf-flowvis.git  
cd mpas-decaf-flowvis  
git submodule update --init  
git submodule update --remote  

## Download MPAS-O + Decaf test directory

mkdir testcases  
cd testcases  
wget https://sandbox.zenodo.org/record/239092/files/MPAS-O_V6.0_QU240_decaf_flowvis.tar.gz  
tar -xvzf MPAS-O_V6.0_QU240_decaf_flowvis.tar.gz  

testdir=path/to/MPAS-O_V6.0_QU240_decaf_flowvis  
homedir=path/to/mpas-decaf-flowvis/home/directory  


## Build Decaf
cd $homedir/decaf  
mkdir build install  
cd build  
ccmake .. -DCMAKE_INSTALL_PREFIX:PATH=../install -Dtransport_cci=off -DVTK_DIR=/path/to/vtk/install -Dlibnabo_DIR=/path/to/libnabo/install
make -j 8  
make install  

- Copy executable from mpas_vis directories to testdir 

cp $homedir/decaf/build/examples/mpas_vis/flow_main $testdir   

## Build MPAS

- Set environment variables

export DECAF_FOLDER=$homedir/decaf  
export BOOST_INSTALL_DIR=/path/to/boost/install/directory  
export NETCDF=path/to/NETCDF/install/directory  
export PNETCDF=path/to/PNETCDF/install/directory  
export PIO=path/to/PIO/install/directory

cd $homedir/MPAS-Model  
make clean gfortran CORE=ocean

- Copy MPAS executable to testdir  

cp $homedir/MPAS-Model/ocean_model $testdir

## Run test case

cd $testdir  
python generate_script.py  
./mpas\_decaf\_flowvis.sh


