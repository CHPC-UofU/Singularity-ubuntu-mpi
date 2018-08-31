# Defines a Singularity container with IB stack 
# Includes Lmod for using CHPC installed compilers and MPI distros
# (MPICH derived MPI distros work)



BootStrap: docker
From: ubuntu:16.04
#Include: rpm2cpio


%runscript


%setup
    # Runs from outside the container during Bootstrap



%post
    # Runs within the container during Bootstrap
    # need to create mount point for home dir
    mkdir /uufs
    mkdir /scratch


    # Install the necessary packages (from repo)
    apt-get update && apt-get install -y --no-install-recommends \
        apt-utils \
        build-essential \
        curl \
        git \
        libopenblas-dev \
        libcurl4-openssl-dev \
        libfreetype6-dev \
        libpng-dev \
        libzmq3-dev \
        python-pip \
        pkg-config \
        python-dev \
        python-setuptools \
        rsync \
        software-properties-common \
        unzip \
        vim \
        zip \
        zlib1g-dev
    apt-get clean

    # Set up some required environment defaults
    #MC issue with locale (LC_ALL, LANGUAGE), to get it right:
    apt-get install -y language-pack-en
    locale-gen "en_US.UTF-8"
    dpkg-reconfigure locales
    export LANGUAGE="en_US.UTF-8"
    echo 'LANGUAGE="en_US.UTF-8"' >> /etc/default/locale
    echo 'LC_ALL="en_US.UTF-8"' >> /etc/default/locale

    # Update to the latest pip (newer than repo)
   #pip install --no-cache-dir --upgrade pip
   #
   ## Install other commonly-needed packages
   #pip install --no-cache-dir --upgrade \
   #    future \
   #    matplotlib \
   #    scipy 

    
    # IB stuff, based on https://community.mellanox.com/docs/DOC-2431
    apt-get install -y dkms infiniband-diags libibverbs* ibacm librdmacm* libmlx4* libmlx5* mstflint libibcm.* libibmad.* libibumad* opensm srptools libmlx4-dev librdmacm-dev rdmacm-utils ibverbs-utils perftest vlan ibutils
    apt-get install -y libtool autoconf automake build-essential ibutils ibverbs-utils rdmacm-utils infiniband-diags perftest librdmacm-dev libibverbs-dev libmlx4-1 numactl libnuma-dev autoconf automake gcc g++ git libtool pkg-config
    apt-get install -y libnl-3-200 libnl-route-3-200 libnl-route-3-dev libnl-utils

    # git, wget
    apt-get install -y git wget mpich


    # LMod
    apt-get install -y liblua5.1-0 liblua5.1-0-dev lua-filesystem-dev lua-filesystem lua-posix-dev lua-posix lua5.1 tcl tcl-dev lua-term lua-term-dev lua-json

    echo "
if [ -f /uufs/chpc.utah.edu/sys/etc/profile.d/module.sh ]
then
   . /uufs/chpc.utah.edu/sys/etc/profile.d/module.sh
fi
   " > /etc/profile.d/91-chpc.sh

    echo "
. /etc/profile.d/91-chpc.sh
" >> /etc/bash.bashrc


%environment    
    # path to mlx IB libraries in Ubuntu
LD_LIBRARY_PATH=/usr/lib/libibverbs:$LD_LIBRARY_PATH
%test
    # Sanity check that the container is operating
    # none of this will work because sys branch is not bound at bootstrap
#    /uufs/chpc.utah.edu/sys/installdir/intel/compilers_and_libraries_2017.0.098/linux/mpi/intel64/bin/mpirun -np 2 /uufs/chpc.utah.edu/sys/installdir/intel/compilers_and_libraries_2017.0.098/linux/mpi/intel64/bin/IMB-MPI1
   # ml intel
