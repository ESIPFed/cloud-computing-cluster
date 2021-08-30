# Case Studies!

Links to example code!

* [Compare four ways to read COAWST coastal model output](https://gist.github.com/rsignell-usgs/b6bd959712547101acd8ac3ddaf2daab): Gist by @rsignell-usgs compares performance of opening and computing the mean across NetCDF4 and Zarr S3 file stores using h5netcdf or fsspec libraries.

* [NWM ReferenceFileSystem JSON](https://nbviewer.jupyter.org/gist/rsignell-usgs/011e4de62e8db366bb6505c0d4c48ca6): Jupyter Notebook by @rsignell-usgs creates ReferenceFileSystem JSON file for a collection of NWM NetCDF files on S3.

* [Read NWM Forecast "Best Time Series"](https://nbviewer.jupyter.org/gist/rsignell-usgs/649cdce96bfad15dea68fffc5925060e): Jupyter Notebook by @rsignell-usgs uses fsspec's [ReferenceFileSystem](https://filesystem-spec.readthedocs.io/en/latest/api.html#fsspec.implementations.reference.ReferenceFileSystem) to read data from 300 NWM Forecast netcdf files on AWS Public Dataset as a single virtual dataset. Uses `tau=0` for each forecast in the past + the latest forecast to construct the best time series virtual dataset. The only new file we created was the JSON file that points to the original NetCDF files, reads that JSON with the Zarr library.

* [Pangeo Tutorial materials for AGU Ocean Sciences 2020](https://github.com/pangeo-gallery/osm2020tutorial): This repository tutorial materials for a 30 min workshop that showcase Pangeo data analysis on the cloud.
