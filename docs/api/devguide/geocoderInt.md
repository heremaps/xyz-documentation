
# Geocoder Preprocessor

> #### Note
>
> Your account needs access to the Data Hub Add-on Services.

## Capabilities

The Geocoder Preprocessor supports seamless integration of HERE Geocoder service functionalities in a Data Hub space, meaning a user can leverage HERE Geocoder functionality without having to explicitly call the service by themselves.
The Geocoder Preprocessor supports both forward and reverse geocoding.

Forward geocoding functionality is based on the string-based search interface of the HERE geocoder service.
Users need to configure a list of fields that are concatenated to form the search string passed to the geocoder service.
The object is automatically updated with the new coordinates from the geocoder. Reverse geocoding works with point objects only. The coordinates of the point are passed to the reverse geocoder service and the result is appended to the original object in a user-configurable field.

> #### Note
>
> It is actually possible to call forward AND reverse geocoding on the same object in one go. In this case, reverse geocoding will be done before forward geocoding.

Finally, the preprocessor can intelligently defer geocoding to an asynchronous process for cases where lots of objects are written in a single request and might cause request timeouts if processed synchronously. This is done automatically and without need for the user to take action.

## Space Configuration

To activate the preprocessor, just add it to your space configuration.

> #### Note
>
> You need an API Key that enables you to access the HERE Geocoder service.*

``` javascript
{
  "title": "Forward Geocoder Integration",
  "processors": {
    "geocoder-preprocessor": [
      {
        "params": {
          "doReverseGeocode": (true|false), // Enable reverse geocoding (Default=false)
          "doForwardGeocode": (true|false), // Enable forward Geocoding (Default=false)
          "forceForwardGeocoding": (true|false), // Force forward geocoding even if object already has coordinates (Default=false)
          "forwardSaveCoords": (true|false), // Save old coordinates in case they're being overwritten. (Default=false)
          "forwardQuery":["<FIELD_1>","<FIELD_2>",...,"<FIELD_N>"], // List of property keys used to create forward Geocoding search string (Default is empty!)
          "apiKey":"<YourAPIKey>" // API-Key used to authenticate and authorize call to geocoder service. **REQUIRED**
        }
      }
    ]
  }
}
```

At the very least, a user should enable either "```doReverseGeocode```" or "```doForwardGeocode```".
In the case of forward geocoding, at least the "forwardQuery" has to be configured as well. There is no default, and forward geocoding will not work without it.

## Reverse Geocoding

Reverse geocoding is enabled by setting the parameter ```"doReverseGeocode":true``` in the space configuration.

Once enabled, the preprocessor will call the HERE Reverse Geocoder service in order to find out address data for the coordinates of the Point object every time a Point object is written/updated in the space.
The address data returned by HERE Reverse Geocoder will be adopted into the object written into the Data Hub space under the reserved namespace ```"@ns:com:here:xyz:geocoderProcessor"``` on the object's top level. After a successful reverse geocoding call, the user will find a field ```"reverseGCResult"``` within that namespace containing the result from HERE Reverse Geocoder.

> #### Note
>
> HERE Reverse Geocoder will usually deliver several possible address matches, ordered from best match to worst. The Geocoder Preprocessor will only adopt the best match returned by HERE Reverse Geocoder in order to not unnecessarily bloat the resulting object.

## Forward Geocoding

Forward geocoding is enabled by setting the parameter ```"doForwardGeocode":true``` in the space configuration.
In addition, the user will need to provide a list of field names and/or String literals under the ```"properties"``` section of the space's objects.

Example minimum space configuration:

```json
{
  "title": "<SPACE_TITLE>",
  "processors": {
    "geocoder-preprocessor": [
      {
        "params": {
             "doForwardGeocode":true,
             "forwardQuery":["$name", "$municipality", "$iso_country","$iata_code"],
             "apiKey":"<YourAPIKey>"
        }
      }
    ]
  }
}
```

This configuration assumes that the address data to be used for forward geocoding is found in the properties nested under the key ```"properties"```.
The object structure might look something like this:

``` json
  {
      "type": "Feature",
      "properties": {
        "id": "5781",
        "ident": "SAEZ",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "elevation_ft": "67",
        "continent": "SA",
        "iso_country": "AR",
        "iso_region": "AR-B",
        "municipality": "Buenos Aires",
        "scheduled_service": "yes",
        "gps_code": "SAEZ",
        "iata_code": "EZE"
      }
    }
  ...
```

> #### Note
>
> ##### forwardQuery
>
> Only values residing within the ```"properties"``` part of an object can be used for forward geocoding. Therefore, "properties." must be omitted from the field names listed in the ```"forwardQuery"```.
>
> ##### Nested Properties
>
> Nested properties are referenced by standard javascript Dot-notation, i.e. components of the path leading to the referenced value are simply concatenated by a ".", for example: ```"$street.name"```.
>
> ##### Strings and Identifiers
>
> In order for the preprocessor to recognize a string from the list as a field identifier, it must begin with a dollar sign ("$"). Otherwise the string will be interpreted as a string literal and simply be concatenated to the search string.

For this example object, the search string passed to HERE Forward Geocoder would look like this: ```"Ministro Pistarini International Airport Buenos Aires AR EZE"```.

### Further parameters

+ ```"forceForwardGeocoding"``` : If this parameter is present and set to boolean true, the Geocoder Preprocessor will perform geocoding on an object regardless of whether that object already has coordinates or not. During this operation, the object will be of type "Point" and have its coordinates set to the result from HERE Forward Geocoder call.
+ ```"forwardSaveCoords"``` : If this parameter is present and set to boolean true, any geometry that might be overwritten by a call will be saved under the key ```"previousCoordinates"``` within the top-level namespace ```"@ns:com:here:xyz:geocoderProcessor"``` so they will not be lost.

   > #### Note
   >
   > ##### Prerequisite
   >
   > This parameter will only take effect if ```"forceForwardGeocoding"``` is also enabled!
   >
   > ##### Only one
   >
   > Only the previous set of coordinates will be saved in order to prevent unnecessary object bloat!

+ ```"verbosity"``` : Can be set to ```"none"```, ```"min"```, ```"more"``` or ```"all"``` and signifies how much of the original response from geocoder shall be returned as part of the object when forward geocoding. With ```"none"```, only the coordinates returned by the geocoder are used. ```"min"``` will additionally provide scoring information, title and resultType information. ```"more"``` will add the complete address information and ```"all"``` will include the complete geocoder result object.

### Asynchronous processing of Features

When the preprocessor decides a request should be processed asynchronously in order to prevent it from timing out, the objects selected for asynchronous processing will be flagged. The flag can be found in the ```"properties"``` section of the object, within the reserved namespace ```"@ns:com:here:xyz:geocoderProcessor"```. While the object is waiting for async processing, there will be a field ```"async"``` with a value of ```"pending"```. Users can search for this field in order to discern whether there are unprocessed objects present in the space.

## Demo

In this demo we will use two data sets. One has the airport names and municipalities but no coordinates, the other coordinates but no names.  
Source for the airport data is <https://ourairports.com/data/>. The data was converted to JSON and reduced to just 5 airports.

### Preparation

Create a [Data Hub token](../getting-token.md) with access to all spaces and at least Geocoder processor.  
Create an ApiKey in Developer Portal.

```sh
token=<YourDataHubToken>
apiKey=<YourAPIKey>
```

They are referenced in the respective curl calls in the following text.

### Geocode Airport addresses

Have a look at the [airport_addresses.json](./airport_addresses.geojson)
file. One record looks like this:

```json
  {
      "type": "Feature",
      "properties": {
        "id": "5781",
        "ident": "SAEZ",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "elevation_ft": "67",
        "continent": "SA",
        "iso_country": "AR",
        "iso_region": "AR-B",
        "municipality": "Buenos Aires",
        "scheduled_service": "yes",
        "gps_code": "SAEZ",
        "iata_code": "EZE"
      }
    }
```

We need to construct a most accurate address by combining the properties.

#### Create Space

Now create a Space with Geocoder enabled. This is the Space config:

```json
{
  "title": "Geocoder Demo",
  "description": "Upload Airports without geometry and add with Geocoder",
  "processors": {
    "geocoder-preprocessor": [
      {
        "params": {
             "doForwardGeocode":true,
             "forwardQuery":["$name", "$municipality", "$iso_country","$iata_code"],
             "apiKey":"<YourAPIKey>"
        }
      }
    ]
  }
}
```

Here the API call with curl (jq is only for formatting output. You don't need that. You can download the referenced Geocoder_createSpace_req.json [here](Geocoder_createSpace_req.json).

```sh
curl -s -X POST "https://xyz.api.here.com/hub/spaces" -H  "Authorization: Bearer ${token}" -H  "accept: application/json" -H  "Content-Type: application/json" -d @Geocoder_createSpace_req.json | jq '.'
```

The response will look like this:

```json
{
  "id": "aAslJFQ9",
  "title": "Geocoder Demo",
  "description": "Upload Airports without geometry and add with Geocoder",
    "processors": {
      "storage": {
      "id": "psql-db1-eu-west-1",
      "params": null
    },
    "processors": {
      "geocoder-preprocessor": [
        {
          "params": {
            "doForwardGeocode": true,
            "forwardQuery": [
              "$name",
              "$municipality",
              "$iso_country",
              "$iata_code"
            ],
            "apiKey": "<YourAPIKey encrypted>",
            "asyncThreshold": 200,
            "maxParallelRequests": 50,
            "verbosity": "MIN"
          },
          "eventTypes": [
            "ModifyFeaturesEvent.request",
            "ModifyFeaturesEvent.response",
            "ModifySpaceEvent.request"
          ]
        }
      ]
    },
  ...
}
```

Note that the ApiKey is encrypted now. You can't extract it again from this.

Note down and store in a variable the Space id. In this example "aAslJFQ9".

```sh
space=aAslJFQ9
```

#### Upload Data

Upload the file [airport_addresses.json](./airport_addresses.geojson)

```sh
curl -s -X PUT "https://xyz.api.here.com/hub/spaces/${space}/features" -H  "accept: application/geo+json" -H  "Authorization: Bearer ${token}" -H  "Content-Type: application/geo+json" -d @airport_addresses.geojson | jq '.'
```

As the files contains less than 200 features, the geocoding happens synchronously with the call. You see the result right away in the response. The Airport in Buenos Aires now looks like this:

```json
  {
      "type": "Feature",
      "id": "uyw37yYWIFdFIRVi",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -58.54114,
          -34.81372
        ]
      },
      "properties": {
        "@ns:com:here:xyz": {
          "space": "aAslJFQ9",
          "createdAt": 1608293817104,
          "updatedAt": 1608293817104,
          "tags": []
        },
        "continent": "SA",
        "iso_country": "AR",
        "iata_code": "EZE",
        "ident": "SAEZ",
        "elevation_ft": "67",
        "municipality": "Buenos Aires",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "iso_region": "AR-B",
        "id": "5781",
        "gps_code": "SAEZ",
        "scheduled_service": "yes",
        "@ns:com:here:xyz:geocoderIntegration": {
          "lastForwardGCSearch": " Ministro Pistarini International Airport Buenos Aires AR EZE",
          "gcResponse": {
            "title": "Aeropuerto Internacional de Ezeiza",
            "resultType": "place",
            "categories": [
              {
                "id": "400-4000-4581",
                "name": "Aeropuerto",
                "primary": true
              }
            ],
            "scoring": {
              "queryScore": 0.58,
              "fieldScore": {
                "country": 1,
                "state": 1,
                "placeName": 0.69
              }
            }
          }
        }
      }
    }
```

Beside the coordinates there is a new namespace geocoderIntegraton. This has the last search string used. If you update the feature and the search string doesn't change, we won't call the geocoder again. If the search string changes, we will.

#### View Result

You can have a look at the full data set with Geojson Tools. Add your data to the following link: <http://geojson.tools/?url=https://xyz.api.here.com/hub/spaces/<Your Space Id>/iterate?access_token=<YourToken>>

### Reverse Geocode the HERE office location

Source file for this is "airport_points.geojson". One record looks like this:

```json
  {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -58.5358,
          -34.8222
        ]
      },
      "properties": {
        "id": "5781",
        "ident": "SAEZ",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "elevation_ft": "67",
        "continent": "SA",
        "iso_country": "AR",
        "iso_region": "AR-B",
        "municipality": "Buenos Aires",
        "scheduled_service": "yes",
        "gps_code": "SAEZ",
        "iata_code": "EZE"
      }
    }
```

We have the location, name and the city, but no full address.

#### Create Space

Create a Space with Reverse Geocoder enabled. This is the Space config:

```json
{
  "title": "Reverse Geocoder Demo",
  "description": "Upload HERE Offices with geometry and add address with Geocoder",
  "processors": {
    "geocoder-preprocessor": [
      {
        "params": {
             "doReverseGeocode":true,
             "apiKey":"<YourAPIKey>"
        }
      }
    ]
  }
}
```

Here the API call with curl (jq is only for formatting output. You don't need that. You can download the referenced ReverseGeocoder_createSpace_req.json file [here](ReverseGeocoder_createSpace_req.json))

```sh
curl -s -X POST "https://xyz.api.here.com/hub/spaces" -H  "Authorization: Bearer ${token}" -H  "accept: application/json" -H  "Content-Type: application/json" -d @ReverseGeocoder_createSpace_req.json | jq '.'
```

The response will look like something like this:

```json
{
  "id": "afCyNAvu",
  "title": "Reverse Geocoder Demo",
  "description": "Upload Airports with geometry and add address with Geocoder",
  "storage": {
    "id": "psql-db1-eu-west-1",
    "params": null
  },
  "processors": {
    "geocoder-preprocessor": [
      {
        "params": {
          "doReverseGeocode": true,
          "apiKey": "<YourAPIKeyEncrypted>"
        },
        "eventTypes": [
          "ModifyFeaturesEvent.request",
          "ModifySpaceEvent.request"
        ]
      }
    ]
  },
  "createdAt": 1598016894262,
  "updatedAt": 1598016894262,
  "contentUpdatedAt": 1598016894262
}
```

Again the ApiKey is encrypted and we note the Space id "afCyNAvu".

```console
space=afCyNAvu
```

#### Upload Data

Upload the attached file ["airports.geojson"](airports.geojson).

```sh
curl -s -X PUT "https://xyz.api.here.com/hub/spaces/<YourSpaceId>/features" -H  "accept: application/geo+json" -H  "Authorization: Bearer <YourToken>" -H  "Content-Type: application/geo+json" -d @airports.geojson | jq '.'
```

This file also has  less then 200 features, and we can see the result of the reverse geocoding right away. The airport in Buenos Aires now looks like this:

```json
   {
      "type": "Feature",
      "id": "vs7LzAxrFYwZQCJ7",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -58.5358,
          -34.8222
        ]
      },
      "properties": {
        "@ns:com:here:xyz": {
          "space": "RbfCcVh5",
          "createdAt": 1608294256979,
          "updatedAt": 1608294256979,
          "uuid": "d0493165-3d9c-46d7-ae79-29e662b5fa76",
          "tags": []
        },
        "continent": "SA",
        "iso_country": "AR",
        "iata_code": "EZE",
        "ident": "SAEZ",
        "elevation_ft": "67",
        "municipality": "Buenos Aires",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "iso_region": "AR-B",
        "id": "5781",
        "gps_code": "SAEZ",
        "scheduled_service": "yes",
        "@ns:com:here:xyz:geocoderIntegration": {
          "lastRevGClon": -58.5358,
          "lastRevGClat": -34.8222,
          "reverseGCResult": {
            "title": "Aeropuerto Ezeiza, Ezeiza, Buenos Aires, Argentina",
            "id": "here:cm:namedplace:24211314",
            "resultType": "locality",
            "localityType": "district",
            "address": {
              "label": "Aeropuerto Ezeiza, Ezeiza, Buenos Aires, Argentina",
              "countryCode": "ARG",
              "countryName": "Argentina",
              "stateCode": "BA",
              "state": "Buenos Aires",
              "county": "Ezeiza",
              "city": "Ezeiza",
              "district": "Aeropuerto Ezeiza",
              "postalCode": "1802"
            },
            "position": {
              "lat": -34.78927,
              "lng": -58.52403
            },
            "distance": 0,
            "mapView": {
              "west": -58.58973,
              "south": -34.84987,
              "east": -58.50827,
              "north": -34.74075
            }
          }
        }
      }
    }
```

In the namespace geocoderIntegration you will now find the response from Geocoder which contains beside the address more information.

#### View Result

You can have a look at the full data set with Geojson Tools. Here the Space from demo video <http://geojson.tools/?url=https://xyz.api.here.com/hub/spaces/afCyNAvu/iterate?access_token=AEdt5QItR4Wuw3k-vHak_gA>

## Epilog

Let's compare the Frankfurt office in the two Spaces side by side:

<table>
<tr>
<th>Geocoder</th><th>Reverse Geocoder</th><th>Note</th></tr>
<tr>
<td valign="top"><code style="white-space:pre"><pre>{
      "type": "Feature",
      "id": "uyw37yYWIFdFIRVi",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -58.54114,
          -34.81372
        ]
      },
      "properties": {
        "@ns:com:here:xyz": {
          "space": "aAslJFQ9",
          "createdAt": 1608293817104,
          "updatedAt": 1608293817104,
          "tags": []
        },
        "continent": "SA",
        "iso_country": "AR",
        "iata_code": "EZE",
        "ident": "SAEZ",
        "elevation_ft": "67",
        "municipality": "Buenos Aires",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "iso_region": "AR-B",
        "id": "5781",
        "gps_code": "SAEZ",
        "scheduled_service": "yes",
        "@ns:com:here:xyz:geocoderIntegration": {
          "lastForwardGCSearch": " Ministro Pistarini International Airport Buenos Aires AR EZE",
          "gcResponse": {
            "title": "Aeropuerto Internacional de Ezeiza",
            "resultType": "place",
            "categories": [
              {
                "id": "400-4000-4581",
                "name": "Aeropuerto",
                "primary": true
              }
            ],
            "scoring": {
              "queryScore": 0.58,
              "fieldScore": {
                "country": 1,
                "state": 1,
                "placeName": 0.69
              }
            }
          }
        }
      }
    }</pre></code></td>
<td valign="top"><code style="white-space:pre"><pre> {
      "type": "Feature",
      "id": "vs7LzAxrFYwZQCJ7",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -58.5358,
          -34.8222
        ]
      },
      "properties": {
        "@ns:com:here:xyz": {
          "space": "RbfCcVh5",
          "createdAt": 1608294256979,
          "updatedAt": 1608294256979,
          "uuid": "d0493165-3d9c-46d7-ae79-29e662b5fa76",
          "tags": []
        },
        "continent": "SA",
        "iso_country": "AR",
        "iata_code": "EZE",
        "ident": "SAEZ",
        "elevation_ft": "67",
        "municipality": "Buenos Aires",
        "type": "large_airport",
        "name": "Ministro Pistarini International Airport",
        "iso_region": "AR-B",
        "id": "5781",
        "gps_code": "SAEZ",
        "scheduled_service": "yes",
        "@ns:com:here:xyz:geocoderIntegration": {
          "lastRevGClon": -58.5358,
          "lastRevGClat": -34.8222,
          "reverseGCResult": {
            "title": "Aeropuerto Ezeiza, Ezeiza, Buenos Aires, Argentina",
            "id": "here:cm:namedplace:24211314",
            "resultType": "locality",
            "localityType": "district",
            "address": {
              "label": "Aeropuerto Ezeiza, Ezeiza, Buenos Aires, Argentina",
              "countryCode": "ARG",
              "countryName": "Argentina",
              "stateCode": "BA",
              "state": "Buenos Aires",
              "county": "Ezeiza",
              "city": "Ezeiza",
              "district": "Aeropuerto Ezeiza",
              "postalCode": "1802"
            },
            "position": {
              "lat": -34.78927,
              "lng": -58.52403
            },
            "distance": 0,
            "mapView": {
              "west": -58.58973,
              "south": -34.84987,
              "east": -58.50827,
              "north": -34.74075
            }
          }
        }
      }
    }</pre></code></td>
<td>The adress is exatly the same.
The coordines are slightly different.
The Reverse Geocoder returnes beside the adress also coordines to access the building, a map view and a position. This position is exactly what we got from forward geocoder.</td>
</tr>
</table>
