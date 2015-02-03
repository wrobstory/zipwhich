# zipwhich

An attempt to create one zip/city/state/lat/lon dataset to rule them all.

We're going to do a comparo of three different zip code datasets to see how well they match zip, city, and lat/lon. The
three datasets are:

* GeoNames: http://download.geonames.org/export/zip/ http://federalgovernmentzipcodes.us/ noncensus:
* https://github.com/ramhiser/noncensus

We're also going to use the [TwoFishes](http://twofishes.net/) geocoder to see if we can get more accurate lat/lon data.

### Why?

Because I couldn't find a holy-grail dataset with those dimensions that seemed 100% complete. Zipcodes are hard.
Locations are hard.  I wanted to see if I could come to a consensus from the freely available data.

### How

The steps are mostly laid out in the Jupyter/IPython Notebook. Briefly:

* Import all datasets, check number of unique zipcodes in each
* Outer join the dataset with the most unique zips (GeoNames) to each of the other two on Zip + State
* Inner join those two joined datasets to one another
* Attempt to find a primary city/state based on how well the three datasets agree. If there's no agreement, fall-back to noncensus.
* Based on the primary city/state, query TwoFishes to get a fourth set of lat/lon from a production geocoder.
* Given our four sets of lat/lon, get the min/max Haversine distance between each pair * Rank the lat/lon is order of trustworthyness: TwoFishes, Noncensus, GeoNames, FreeZip
* If the min distance between TwoFishes and noncensus is < 1km, use TwoFishes. Otherwise, use the better-ranked of the min-distance pair
* Two final datasets:
    - distinct_cities, which has a row for each city/state (some states have cities of the same name). Lat/Lon are centroids of all the zips within that city
    - distinct_zips, which has a row for each zip code
