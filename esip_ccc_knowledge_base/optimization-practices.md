# Optimization Practices

## Chunking
Cloud-performant data is all about the chunks and less about the format.  You just need  consolidated metadata, a reasonable chunk size, reasonable compression, and attention to your chunk shape relative to expected access patterns. 

The actual format you use matters less. Because the cloud supports many concurrent access from the same storage object, reading chunks from separate  objects (e.g. sharded approaches like Zarr) or reading chunks via byte-ranges from a single object performs about the same. 

Issues that need addressing: 
* write performance -- clearly parallel write is easier with sharded formats like Zarr. 
* representing data cubes using formats like COG, which force a dimension of 1 in the time dimension?

For many file formats, including HDF, NetCDF, and Zarr, chunks constitute the smallest unit of data within a file that can be read at once, as they are typically compressed.  Reading a chunk of data incurs a latency cost, so chunk sizes ought not be too small or performance will suffer. 

Reading a chunk of data also incurs a data transfer and decoding cost, and entire chunks must be read (if they are compressed).  If chunks are too large (for example, one chunk for the entire dataset), there will be too much data transfer and decoding for common use cases, and bytes may need to be stored in slower memory locations.  

Regarding chunk shape, it should be chosen to optimize performance for expected access patterns.  The optimal chunk size shape depends on expected access, but it also varies with the latency and throughput of the data store. For data in cloud storage, the latter characteristics can be dramatically different from data stored on a local disk. Chunk size for cloud stores therefore needs careful consideration and cannot easily rely on non-cloud rules of thumb.

### Chunk Size

A chunk size should be selected that is large in order to reduce the number of tasks that parallel schedulers like Dask have to think about (which affects overhead) but also small enough so that many of them can fit in memory at once. The Pangeo project has been recommending a chunk size of about 100MB, which originated from the Dask Best Practices. The Zarr Tutorial recommends a chunk size of at least 1MB. The Amazon S3 Best Practices says the typical size for byte-range requests is 8-16MB. It would seem that chunk sizes on the order of 10MB or 100MB are most optimal for Cloud usage.


### Chunk Compression and Filtering

The particular compression scheme is less important than just having some kind of compression.  ZLIB (DEFLATE) is probably the most commonly used, mostly because it’s widely supported by software.   Some schemes are clearly better than ZLIB for certain use cases, however, and many are supported in the Blosc compression library ( written in C and therefore available in languages like Python, R, Java, Julia).  The default Blosc compression scheme used by Xarray when writing Zarr is the “LZ4” scheme, a result of the benchmarking reported in a blog post by Alistair Miles, where he said “for speed you can’t beat LZ4, but Zstd+Bitshuffle gives a very high compression ratio with good all-round performance default compression scheme etc.”   

### Chunk Shape
Once the chunk size is chosen, the chunk shape should be chosen to minimize the number of chunks that need to be read for expected access patterns.  For example, the optimal chunk shape for a time stack of remote sensing images (a data cube) might be rods with many points in the time dimension and few points in the spatial dimensions if time series extraction is the dominant expected access pattern.   If both time series extraction and spatial map extraction at fixed points in time are common expected access patterns, the chunk shape might be picked so the number of chunks extracted in each case is the same. 

## Data Management Considerations

### Provenance and Data Citation
One complication of reformatting data into a cloud optimized format is tracking data provenance back to the source data. This is particularly an issue in citing data in science data papers. Ideally, the source data should be referenced, to provide proper credit to the data provider and because documents describing the data and its production (e.g. Algorithm Theoretical Basis Documents) typically refer to the source data. However, it would also be desirable to note that data used in a study or application was in fact the reformatted / restructured dataset. 

Key questions are:

* Should derived and original data have their own DOIs?  Ideally yes.
* Are there any patterns for maintaining the linkage?  The STAC derived-from link may serve as a model.

Remaining questions are:

* How to deal with reformatted datasets that are not exactly the same as the source data?

## Data Discovery

Some approaches to cloud optimized data may force a re-evaluation of data discovery. Many current data systems provide discovery for file-based archives that includes a search for files matching a user’s region of interest. However, some data reorganizations into optimized structures and formats (e.g., Zarr, Entwine Point Tiles) can abstract the file arrangement from the users when aggregated over one or more dimensions, such as time. This has the benefit of providing a simpler data domain for the user to navigate and access, but also renders file-based searching irrelevant to the discovery process. (However, file-based searching may be relevant in the provenance aspect.)

## Storage Tiers

As datasets reach peta-scale size and growth across many sciences, the expense of permanent storage versus immediate accessibility becomes problematic.  A recent paper (Carrasco-Kind, et al, 2020: http://hdl.handle.net/2142/107186) discussed the implementation of a ‘carousel’ model of data access that permitted bulk recovery of cold-stored datasets on inexpensive archival storage to be staged to available disk through asynchronous standing orders for data.  This is one of perhaps many practical solutions that technologists are thinking about right now as they plan to move their data to the cloud.

Surely, many data repositories moving to the cloud will need to consider having at least two tiers of object store to ensure cost-effectiveness, availability, and durability of scientific data.  The primary tier will be an always-available, on-disk object store that is synchronously accessible.  The secondary tier would be a form of cold store that allows time-delayed access to datasets, pushed to a primary data tier for a limited time to allow for availability to requesters.

Beyond archival data stores, a cloud platform will need to consider the support of more mutable forms of storage, some persistent (databases) and others transient (a session work space).  Mutable data stores, whether file store or block store, serve to support the curation and dissemination of data that is kept resident in scalable object storage.  The size and lifetime of these data stores must be given careful consideration as their cost of usage greatly outsizes that of object storage.
