# Antipatterns (e.g. what doesn’t work)

A community survey of the ESIP Cloud Computing Cluster noted the following antipatterns in cloud data:

* Large granule sizes with no means to subset those granules, requiring full-file transfers.
* Storing data uncompressed, which can increase performance but double cost.
* Lift and shift, which fails to make use of the cloud’s elasticity and highly scalable storage while also ignoring changes in access performance.
* Large central file mounts, which create scalability and performance issues
* Persistent processes, especially inelastic one. Scientific workloads tend to have high variability in demand and require attention to scaling to provide burst performance without inordinate cost
* Requiring many requests in order to read a file, e.g. due to chunk sizes that are too small compared to the overall access size, which create significant network chatter
* Web-based portals instead of direct programmatic data access, which limits automated activity and scalability while returning too many results
* Failing to cache for repeat access, both client- and server-side, and particularly in service outputs which are expensive to produce and need idempotency
* Putting workflows on the client, which creates significant back-and-forth network traffic
* Allowing for unconstrained spatial queries of full detail
* Glaciering data without a defined thawing process, with ownership of associated costs
* Extracting time series from thousands of one-time-step files
* Hierarchical data tree walks, which tend to be slow and prone to runtime errors (e.g. netCDF/HDF groups?)
* Scattering metadata all over the place in the dataset
* Making “one-time” chunking scheme, variable or projection decisions for all end users (seems like this should be configurable for different users and different use cases)
* Data without metadata and metadata APIs: Data is on the cloud but people can’t understand it without downloading it
* Storing related files in zip, tar, or similar formats not well-suited for partial file reads and spatiotemporal access
* Serving data with no provision for partial-file access
* Compressing data with unconventional (non-default, difficult to find/install) compressors.
* Everyone, data producers and data users, needing time to learn new tools and data access methods.
