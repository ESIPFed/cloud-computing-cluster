# Resources for Cloud Data Optimization

## Data Formats

### Cross-Format Evaluations

[Task 51 - Cloud-Optimized Format Study](https://ntrs.nasa.gov/search.jsp?R=20200001178) - A NASA cross-format evaluation on the performance characteristics of several formats in cloud environments.

### [Cloud-Optimized GeoTIFFs (COGs)](https://www.cogeo.org/)

[Landsat Cloud Optimized GeoTIFF Data Format Control Book](https://www.usgs.gov/media/files/landsat-cloud-optimized-geotiff-data-format-control-book): Details on format selections and packaging for Landsat Collection 2.

_COG Talk_ Series, starting with [COG Talk — Part 1: What's new?. This blog is the first in a series… | by Vincent Sarago | Development Seed](https://medium.com/devseed/cog-talk-part-1-whats-new-941facbcd3d1)

### [HDF5](https://support.hdfgroup.org/HDF5/)

[Highly Scalable Data Service (HSDS)](https://github.com/HDFGroup/hsds): A REST-based web service for HDF5 data stores, built by The HDF Group, with several optimizations for data access in network environments.

H5Coro: The Cloud-Optimized Read-Only Library
* [HDF Group Webinar Slides](https://www.hdfgroup.org/wp-content/uploads/2021/05/JPSwinski_H5Coro.pdf)
* [HDF Group Webinar](https://www.youtube.com/watch?v=RuBKDW4TNO4)
* [ICESat-2 SlideRule (Science data processing as a service) H5Coro Documentation](http://icesat2sliderule.org/h5coro/): The most detailed explanation of H5Coro

### Zarr, NetCDF, HDF5

[NetCDF and Native Cloud Storage Access via Zarr](https://www.unidata.ucar.edu/blogs/news/entry/netcdf-and-native-cloud-storage)

[Cloud-Performant Reading of NetCDF4/HDF5 Data Using the Zarr Library](https://medium.com/pangeo/cloud-performant-reading-of-netcdf4-hdf5-data-using-the-zarr-library-1a95c5c92314)


## Storage Guidance

[Performance Guidelines for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/optimizing-performance-guidelines.html): Amazon suggestions to optimize performance on S3.

## Benchmarking

[Pangeo benchmarking](https://github.com/pangeo-data/benchmarking): Benchmarking and scalng studies of the Pangeo Platform.

## Chunking

[Making	earth science data more accessible: experience with chunking and compression](https://www.unidata.ucar.edu/presentations/Rew/ams-2013-rew-fixed.pdf): Presentation by XX of unidata provides explanation of what chunking is, why chunking is important for big data access, and guidance for choosing the right chunk shape.

## Categorical Data Standards

Communities of geospatial data develop data standards in order to facilitate the adoption and sharing of data and code across users and platforms. Cloud data providers may see greater data use if providing data adhering to these standards. Below are some examples of data standards for a scoped category of geospatial data.

[Standard for the Exchange of Earthquake Data (SEED)](http://www.fdsn.org/pdf/SEEDManual_V2.4.pdf)
[Communitee on Earth Observing Satellites (CEOS) Analysis Ready Data (ARD) for Land (CARD4L)](https://ceos.org/ard/)


