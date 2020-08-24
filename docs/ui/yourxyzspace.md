# Display your Data Hub Space content

Adding a `SpaceProvider` as an additional `Layer` to the `display` enables you to quickly show
your content on top of a map, without having to impement any additional functionality to fetch
data from the space every time the user interacts with the map, like panning the map or zooming
in and out. This will be handled completely by the provider/layer combination and the only
thing you need to configure is where the data is you want to show, and at which zoom levels
you want it to be visible.

1. Create and configure a `SpaceProvider` acting as a datasource to your Data Hub Space content

    ```javascript
    // Define provider for this layer
    var mySpaceProvider = new here.xyz.maps.providers.SpaceProvider ({
      // Name of the provider
      name:  'SpaceProvider',

      // Zoom level at which tiles are cached
      level: 14,

      // Space ID
      space: 'playground-link',

      // User credential of the provider
      credentials: {
        access_token: YOUR_ACCESS_TOKEN
      }
    });
    ```

2. Create a Layer for displaying your data provided by your GeoSpaceProvider

    ```javascript
    // Create data layer with Space provider
    var myLayer = new here.xyz.maps.layers.TileLayer({
      // Name of the layer
      name: 'mySpaceLayer',

      // Minimum zoom level
      min: 14,

      // Maximum zoom level
      max: 20,

      // Define provider for this layer
      provider: mySpaceProvider
    })
    ```

3. And now add the Layer to your display

    ```javascript
    display.addLayer( myLayer )
    ```
