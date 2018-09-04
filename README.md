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
ccmake .. -DCMAKE_INSTALL_PREFIX:PATH=../install -Dtransport_cci=off
make -j 8
make install

- Copy shared library and executable from mpas_vis directories to testdir
cp $homedir/decaf/install/examples/mpas_vis/mod_mpas_adapter2.so $testdir
cp $homedir/decaf/build/examples/mpas_vis/flow_main $testdir

## Build MPAS
- Copy shared library from mpas_vis directory to MPAS source folder
cp $homedir/decaf/build/examples/mpas_vis/mod_mpas_adapter2.so  $homedir/MPAS-Model/src/

export DECAF_FOLDER=$homedir/decaf
export BOOST_INSTALL_DIR=/path/to/boost/install/directory

export NETCDF=path/to/NETCDF/install/directory
export PNETCDF=path/to/PNETCDF/install/directory
export PIO=path/to/PIO/install/directory

cd $homedir/MPAS-Model
make clean gfortran CORE=ocean

- Copy ocean_model to testdir 
cp $homedir/MPAS-Model/ocean_model $testdir

## Running example

cd $testdir
python3 generate_script.py
./mpas_decaf_flowvis.sh


