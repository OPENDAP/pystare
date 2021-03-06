# PySTARE

## Introduction
PySTARE exposes the STARE library to python.


## Requirements:
pystare requires STARE to be installed i.e. expects
libSTARE.a in /usr/local/lib/ and STARE.h in /usr/local/include/

    git clone https://github.com/SpatioTemporal/STARE
    cd STARE
    cmake .
    make
    make install

Alternatively, you can build and install like this

    git clone https://github.com/michaelleerilee/STARE
    cd STARE
    mkdir build			# Build the code here, separeate from the source
    mkdir local			# Install the code here, within the STARE tree
    cd cmake .. -DCMAKE_INSTALL_PREFIX=local
    make
    make install

If no rights for make install are present, the location of libSTARE.a
and STARE.h can be specified in setup.py

    library_dirs=[],       # Location of libSTARE.a
    include_dirs=[],       # Location of STARE.h

or by shell environment variables (e.g. in bash):

    export STARE_INCLUDE_DIR=/path/to/directory-containing-stare.h/
    export STARE_LIB_DIR=/path/to/directory-containing-stare.a/

It may be necessary to set PYTHON_INCLUDE_DIRS, if, for example, numpy
headers cannot be found. This is the case on OSX.

You also need to install SWIG. On OSX 'brew install swig'.

STARE version 0.16.0 or greater is required.

## Installation (using virtualenvwrapper[1])

    mkvirtualenv --python=/usr/bin/python3 $PROJECT_ENV    
    pip3 install git+git://github.com/SpatioTemporal/pystare.git

### Or from local copy

    git clone https://github.com/SpatioTemporal/pystare $pystare
    pip3 install --editable $pystare
    
### Manual build
    
    python3 setup.py build_ext --inplace
    python3 setup.py build --force
    
    
## Tests
    python3 setup.py test
    
    
## Usage

Once pystare is installed or made available via PYTHONPATH one may use it as described in the following sections.

### Example scripts

Examples are provided in the examples directory and may be run as follows.

    python3 examples/test_intersect_single_res.py

### Spatial

    import numpy
    import pystare
    
    print('Spatial tests')
    lat = numpy.array([30,45,60], dtype=numpy.double)
    lon = numpy.array([45,60,10], dtype=numpy.double)

    indices = pystare.from_latlon(lat, lon, 12)
    print('0 indices: ', [hex(i) for i in indices])

    lat, lon = pystare.to_latlon(indices)
    print(lat, lon)

    lat, lon, level = pystare.to_latlonlevel(indices)
    print(lat, lon, level)

    level = pystare.to_level(indices)
    print(level)

    area = pystare.to_area(indices)
    print(area)

### Temporal

    import numpy
    import pystare
    
    print('Datetime tests')
    datetime = numpy.array(['1970-01-01T00:00:00', 
                        '2000-01-01T00:00:00', 
                        '2002-02-03T13:56:03.172', 
                        '2016-01-05T17:26:00.172'], dtype=numpy.datetime64)
    print(datetime)
    print(datetime.astype(numpy.int64))
    
    index = pystare.from_utc(datetime.astype(numpy.int64), 6)
    print([hex(i) for i in index])

    index = pystare.from_utc(datetime.astype(numpy.int64), 27)
    print([hex(i) for i in index])

### References

[1] https://virtualenvwrapper.readthedocs.io/en/latest/design.html