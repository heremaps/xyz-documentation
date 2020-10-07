## Data Hub Tools and Integrations

You can acess the Data Hub API using a number of different SDKs and APIs.

### XYZ Spaces for Python

https://github.com/heremaps/xyz-spaces-python

This open source Python package allows to interact with your XYZ spaces and features on a given Hub using a higher level programmatic interface that wraps the RESTful API. Using this package you can:

- Create, read, list, update, share, delete spaces (also: get space info and stats).
- Add, read, update, iterate, search, cluster (hex/quad bins), delete features.
- Search features by ID, tag, property, bbox, tile, radius, geometry.
- Access [add-on features](https://www.here.xyz/cli/datahub_add-on/) like H3 hexbin and quadbin clustering, virtual spaces, extended property search, and more.

#### Installation

This package can be installed with `pip` or `conda` from various sources:

- Install with conda from the Anaconda [conda-forge channel](https://anaconda.org/conda-forge/xyzspaces):

    ```bash
    conda install -c conda-forge xyzspaces
    ```

- Install from the [Python Package Index](https://pypi.org/project/xyzspaces/):

    ```bash
    pip install xyzspaces
    ```

- Install from its source repository on GitHub:

    ```bash
    pip install -e git+https://github.com/heremaps/xyz-spaces-python#egg=xyzspaces
    ```
#### Documentation

The detailed documentation is available [here](https://xyz-spaces-python.readthedocs.io/en/latest/).

### HERE Maps API for Javascript 

Your Data Hub Spaces can be accessed using [HERE MapsJS](https://developer.here.com/documentation/maps/3.1.19.0/dev_guide/topics/xyz-spaces.html
) as of version 3.1.

HERE Maps API for JavaScript provides a simple way to add the data presented as an XYZ space to the existing application. Also, HERE Maps API for JavaScript allows building a new application that uses HERE services, such as Routing, Geocoding, Fleet Telematics Advanced Data Sets and others, alongside with XYZ.

The snippet below:

- Gets an instance of the XYZ service and passes an XYZ access token to it.
- Creates one XYZ provider for each space and adds them as an H.map.layer.TileLayer.

```
// provided that the platform and the map are instantiated.
const service = platform.getXYZService({
  token: <YOUR_XYZ_ACCESS_TOKEN>,
});

// create a provider for the public buildings data
const buildingsSpaceProvider = new H.service.xyz.Provider(service, <XYZ_SPACE_ID>, {
  'min': 14
});
const buildingsSpaceLayer = new H.map.layer.TileLayer(buildingsSpaceProvider);
// add a layer to the map
map.addLayer(buildingsSpaceLayer);

// create a provider for the custom user defined data
const customSpaceProvider = new H.service.xyz.Provider(service, <XYZ_SPACE_ID>);
const customSpaceLayer = new H.map.layer.TileLayer(customSpaceProvider);
// add a layer to the map
map.addLayer(customSpaceLayer);
```

#### Documentation

The API reference provides details on the [Provider](https://developer.here.com/documentation/maps/3.1.19.0/api_reference/H.service.xyz.Provider.html) and [Service](https://developer.here.com/documentation/maps/3.1.19.0/api_reference/H.service.xyz.Service.html) implementations.

