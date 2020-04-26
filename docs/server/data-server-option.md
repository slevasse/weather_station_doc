# Data server options
The data server is the server that will receive the data from the nodes and stores it in the database.

## SQL database/server

### PROs
* Simple to setup
* Many exemples to copy from
* Directly queryable from PHP / web-server

### CONs
* Not suited for scientific data
* Cannot be shared easily

## HDF5 database/server

### PROs
* Simple to setup 
* Simple to setup the server
* Made for scientific data
* No limits in size
* Easily sharable (single file..)
* Easy interface with all scientific languages


### CONs
* (so far from my research) no direct way to query from PHP to visualise on the web-server.
	* For now, will resort to something like PHP calls python script which creates a html file (not very pretty but that should work and enable all kind of pre-processing before display)

