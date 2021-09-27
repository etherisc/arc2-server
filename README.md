# ARC2 Server
REST server for ARC2 rainfall data

## Run the server

She server may be run using the command line or docker

### Command line

Run server on command line

``` bash
python3 server.py
```

### Build Docker Image

Build the docker image

``` bash
docker build -t arc2_server .
```

NOTE regarding docker base image. numpy and other things such as pyproj come with non-trivial dependencies.
alpine as a lightweight base image did not work well. 
The (larger) Ubuntu base image did work.

### Run Server in Docker Container

Run server using docker

``` bash
docker run --name arc2-server -v $PWD/data:/data/arc2 -d -p 5000:5000 arc2_server
```

Run a test server in parallel on port 5001

``` bash
docker run -v $PWD/data:/data/arc2 -d -p 5001:5000 arc2_server
```

## Test the Server

``` bash
curl -X GET "http://localhost:5000/arc2/rainfall?lat=-0.9&long=37.7&date=20200201&days=7"
```

This should produce the following output

```
20200201 19.597993850708008
20200202 0.0
20200203 1.1990208625793457
20200204 0.31660813093185425
20200205 0.40143030881881714
20200206 1.095880150794983
20200207 0.0
```

To check the cache content of the server you may use

``` bash
curl -X GET "http://localhost:5000/arc2/cache?date=20200205&days=4"
```

Which then produces something as follows

```
20200205 /data/arc2/africa_arc.20200205.tif.zip
20200206 /data/arc2/africa_arc.20200206.tif.zip
20200207 /data/arc2/africa_arc.20200207.tif.zip
20200208 initialized
```

Hint: The result will depend on which rainfall data has been accessed already

## Geotiff dependency

geotiff code is by KipCrossing provided with LGPL 2.1 licence
https://github.com/KipCrossing/geotiff
