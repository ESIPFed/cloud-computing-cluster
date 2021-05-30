# Factors Influencing Optimization Decisions

Performance optimization involves reducing the time to locate, retrieve, decode, and prepare data values for analysis.
To optimize for performance, the author should:

* Optimize data location: Optimizing data location implies minimizing the amount of time it takes to determine which locations in which files need to be read in order to fetch the desired data. Different formats have different approaches to this problem, including:
    * Using sidecar or metadata files to describe byte offsets to different variables and spatiotemporal areas within the file
    * Storing byte offsets at predictable locations within the file that can be quickly read
    * Organizing files to allow direct seeks to the data without reading additional offset information
    * Splitting files across a filesystem (or other key/value storage like cloud object stores) so that desired portions of files can be referenced by name
* Optimize data retrieval time: Optimizing data retrieval means minimizing the latency and maximizing the throughput of the system storing the data are as fast as feasible. Assuming that is done, optimization of retrieval means ensuring that desired data can be fetched in the fewest number of reads while reading the smallest amount of extraneous data. (Note that a client may nevertheless choose to parallelize reads for more efficient bandwidth usage or distributed processing.) As these two concerns are highly use-case specific, this needs careful attention.
* Optimize data decoding: Data is often compressed for both cost reasons and transfer performance. Compression schemes and parameters need to balance cost, transfer performance, and decoding performance.
* Optimize preparation for analysis: Once data values have been fetched, are they in the right format, projection, grid, units, etc? Do they have the required quality information, metadata, etc required for valid analysis, and if not, what is required to fetch them? Reducing the number and cost of these steps reduces both the development time to analysis and the raw computation time required for analysis.

Given the above, the answer for how to optimize data will always be "It depends." The primary factors it depends on are:

* Likely access patterns, which are usually driven by likely analysis needs. Data should be arranged such that desired bytes can be located and read in as few requests as possible, while reading as little unnecessary data as possible.
* Latency and throughput of the data store. Object stores tend to be relatively high latency and high throughput. This means that reading extraneous bytes can often be worthwhile if it means making fewer requests. A number to measure and monitor is the number of bytes that could be transferred during the latency period of a request. Below this number, it tends to be better to read extraneous bytes, while above it it tends to be better to perform a separate request.
* End user location and network performance. Similar to the previous item, but on the user side, in-cloud access can have wildly different performance (and cost) characteristics than out-of-cloud access, which themselves can vary greatly.
* Analysis needs. What are users trying to do? What software is readily available to them? What projections, grids (if any), units, etc do they need most? Which data variables are likely to be needed together? What is required to make valid use of science data? Reducing time to analysis means considering these factors, though they are largely out of scope of this document.

While the above optimizes for performance, these concerns need to be balanced against organizational requirements, regulatory requirements, and other factors such as creation, hosting, and transfer cost.
