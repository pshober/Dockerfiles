FROM debian:9.2
RUN apt-get update

# Building GCC
RUN apt-get install --yes wget vim sudo build-essential gfortran

# Building MPICH3
RUN mkdir /software /build
WORKDIR /build
RUN wget http://www.mpich.org/static/downloads/3.1.4/mpich-3.1.4.tar.gz
RUN tar xvf mpich-3.1.4.tar.gz
WORKDIR mpich-3.1.4
RUN bash ./configure 
RUN make -j4 VERBOSE=1
RUN make install
RUN ldconfig
RUN rm -rf /build/*
WORKDIR /build

# Building ACACONDA
RUN wget https://repo.anaconda.com/archive/Anaconda3-5.3.0-Linux-x86_64.sh
RUN bash Anaconda3-5.3.0-Linux-x86_64.sh -b -p /software/anaconda
WORKDIR /
RUN rm -rf /build
WORKDIR /

# So that the anaconda packages including Python3 is on PATH
ENV PATH="/software/anaconda/bin:${PATH}"

# Building MPI4PY linked to container's own MPICH3
RUN env MPICC=/usr/local/bin/mpicc pip install mpi4py
RUN conda remove matplotlib
RUN pip install matplotlib==3.0.0
RUN pip install rebound reboundx
# Looks like we need to update the environment with the location 
# of shared objects used by Python when loading modules
ENV LD_LIBRARY_PATH=/software/anaconda/lib/python3.7/site-packages:${LD_LIBRARY_PATH} 

WORKDIR /

RUN apt-get update 
RUN apt-get install --yes wget vim build-essential gfortran xauth --fix-missing 
