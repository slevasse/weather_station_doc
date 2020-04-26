# HDF5 Database
From wiki : Hierarchical Data Format (HDF) is a set of file formats (HDF4, HDF5) designed to store and organize large amounts of data. 

## What I understand from the hdf5 format
* Files have the `.hdf5` extention
* An HDF5 file is a container for two kinds of objects: datasets, which are array-like collections of data, and groups, which are folder-like containers that hold datasets and other groups. [source](http://docs.h5py.org/en/stable/quick.html#core-concepts)
* A dataset only contains data of a certain data type
* Datasets can e stored contiguously (all data is stored in order on the disk in one block) or by chuncks
	* Chuncks are cool, they allow for compression within the .hdf5 file and to add crc32 error correction/detection
* Metadata is attached to a dataset or groups in the form a "atributes" 
* Apparently you can parse a hdf5 file with SQL commands [see here](https://www.hdfgroup.org/2016/06/hdfql-new-hdf-tool-speaks-sql/)
* Otherwise things looks like a file folder thingy.



## The "node" database requirements

## The "data" database requirements
Here I list what goes in the database.
Depending how I will organise things, the field given bellow can end up as filename, header info or data.

* Unique node ID : A number given (hardcoded in the firmware) to each node in order to identify them. (You can see it as a "key")
* Node firmware version : The version of the firmware used in

## Possible organisation
From most flexible to least flexible. or from least easy to use to easiest to use.
Dataset shall by backed up once a day.

### Option A

* Fixed Nodes(group), mobile node(group) (diferent groups of nodes..)
	* node1(group), node2(group) etc.. (each node has atribute corresponding to itself (location, fw version, hw version...))
		* SensorA(dataset), SensorB(dataset), Timestamp(dataset) (a dataset per sensor (1 x inf) shape) (a timestamp dataset of (1 x inf) shape to store the time and date at the node for each measurement)
			* Each entry in a dataset correspond to a single measurement cycle performed by the node.
			* Each dataset has atributes like:
				* Sensor name/model
				* FW
				* Type of data stored (temp, pressure, etc..))

### Option B
Same as above BUT instead of one dataset per sensor we have one dataset per node.
The dataset is of 2D shape with 1D for each sensor type + time (cols) and 1D for the data collected by the sensor at time T.
The dataset would looks like :

| Time                | temp | pressure | humid |
|---------------------|------|----------|-------|
| 2020-05-03-20-04-27 | 12   | 1000     | 45    |
| 2020-05-03-20-24-27 | 11   | 989      | 42    |

With the header given as attribute.
The time would have to be given as a epoch type thing since only one dtype can be used by dataset.

### Option C
ALL the data is stored in a single dataset 

| Node id | Time                | temp | pressure | humid |
|---------|---------------------|------|----------|-------|
| 1       | 2020-05-03-20-04-27 | 12   | 1000     | 45    |
| 1       | 2020-05-03-20-24-27 | 11   | 989      | 42    |
| 3       | 2020-05-03-20-34-27 | 17   | 967      | 12    |
