## Space configuration
***

This connector provides the user with the possibility to track changes to his space. By activating this feature, every modification of features (insert/update/delete by the **ModifyFeaturesEvent**) is tracked and stored in a separate space.

To activate it, just add the listener to your space.

``` javascript
{
  "id": "2yayumeK",
  "title": "Test space to track changes",
  "listeners": [
    {
      "id": "activity-log",
      "params": {
        "states": 5,            //(Optional) Keeps a maximum of x states per object (x > 0).
        "storageMode": "DIFF_ONLY" | "FULL" | ("FEATURE_ONLY" or <none> -> default)
      }
    }
  ]
}
```

The storage mode decides how the features will be stored.

* FEATURE_ONLY: Will store features with some history relative properties (defined below).
* DIFF_ONLY: Will store features with a 'diff'.'ops' property in the XYZ namespace, containing the RFC-6902 diff to its previous object. The features after the HEAD will only contain the XYZ namespace properties.
* FULL: Will store features with some history relative properties and a 'diff'.'ops' property in the XYZ namespace, containing the RFC-6902 diff to its previous object.

**IMPORTANT**: Since it is not possible to write changes for *<span style="color:red">DeleteFeaturesByTagEvent</span>*, this event type is completely forbidden and the support of this event in the current space is removed.

Within the ModifySpaceEvent, the listener prepares everything and modifies the space to log changes. It will create a new space where the features will be written to, register a new listener that will actually write the modifications and add a processor to prohibit the DeleteByTagEvents.  
Since the ModifyFeatureEvent.response is picked up in a listener, the actual features to log are written asynchronously to the newly created space. As a result, the performance of the actual request is not decreased.

A full space definition with this feature enabled looks like this:

``` javascript
{
  "id": "mySpaceId",
  "title": "Test space to track changes.",
  "description": null,
  "owner": "HERE-12345678-1234-1234-1234-f1c02cc19c37",
  "enableUUID": true,
  "listeners": [
    {
      "id": "activity-log",
      "params": {
        "states": 5,
        "storageMode": "DIFF_ONLY"
      },
      "eventTypes": [
        "ModifySpaceEvent.request"
      ]
    },
    {
      "id": "activity-log-writer",
      "params": {
        "spaceId": "someAuditSpaceId",
        "states": 5,
        "storageMode": "DIFF_ONLY"
      },
      "eventTypes": [
        "ModifyFeaturesEvent.response"
      ]
    },
    ...
  ],
  "processors": [
    {
      "id": "activity-log-writer",
      "eventTypes": [
        "DeleteFeaturesByTagEvent.request"
      ]
    },
    ....
  ]
}
```

## Written features
***
The features written to the new space are stored by uuid. So the space with this feature enabled needs to have **enableUUID** set to true.

Additionally, the original feature element will be slightly modified:

* The id of the feature, will be the **uuid** of the modified feature in the original space.
* The original properties of **'@ns:com:here:xyz'** will be stored under '@ns:com:here:xyz'.**original**
* The original id of the feature will be stored under '@ns:com:here:xyz'.**original**.**id** within the properties.
* A new property will be added under '@ns:com:here:xyz'.**original**.**invalidAt** to indicate the time range, when this object was the HEAD (newest) history object.
* If diff calculation is enabled, an array with differences in accord with RFC-6902 (JSON Patch) will be added under '@ns:com:here:xyz'.**diff**.**ops**.
* The action of the operation (SAVE/UPDATE/DELETE) will be stored under '@ns:com:here:xyz'.**action**
* Additional entries will be added to the tags, like:
* Tracking tags defined by the user (userId, changeId, streamId) //TO BE DONE

After you added a new feature to a space, as follows:

GET {space}/features/1 from original space:

``` javascript
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "1",
      "type": "Feature",
      "properties": {
        "version": 0,
        "@ns:com:here:xyz": {
          "uuid": "0e205bdd-7ed2-4971-87c9-000000000001",
          "space": "LZQ2HELM",
          "createdAt": 1563800857943,
          "updatedAt": 1563800857943
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          1,
          0
        ]
      }
    }
  ]
}
```

... will look like this in audit space:  
(Modifications after creation: The property 'version' was updated, and then the complete feature was deleted.
Depending on the storage mode, the features may or may not contain diffs, in this case DIFF_ONLY.)

``` javascript
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "id": "0e205bdd-7ed2-4971-87c9-000000000001",
      "properties": {
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "EooLq6Z9",
          "original": {
            "id": "1",
            "space": "LZQ2HELM",
            "createdAt": 1563800857943,
            "updatedAt": 1563800857943,
            "invalidAt": 1563800879753
          },
          "action": "SAVE",
          "createdAt": 1563800871858,
          "updatedAt": 1563800885204
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          1,
          0
        ]
      }
    },
    {
      "type": "Feature",
      "id": "0e205bdd-7ed2-4971-87c9-000000000002",
      "properties": {
        "version": 1,
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "EooLq6Z9",
          "original": {
            "id": "1",
            "puuid": "0e205bdd-7ed2-4971-87c9-000000000001",
            "space": "LZQ2HELM",
            "createdAt": 1563800857943,
            "updatedAt": 1563800879753,
            "invalidatedAt": 1563800901213
          },
          "diff": {
            "ops": [
               {
                 "op": "replace",
                 "path": "/properties/version",
                 "value": 0
               }
             ],
             "add": 0,
             "remove": 0,
             "replace": 1,
             "move": 0,
             "copy": 0
          },
          "action": "UPDATE",
          "createdAt": 1563800882833,
          "updatedAt": 1563800882833
        }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          1,
          0
        ]
      }
    },
    {
      "id": "0e205bdd-7ed2-4971-87c9-000000000003",
      "type": "Feature",
      "properties": {
        "@id": "1",
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "EooLq6Z9",
          "original": {
            "updatedAt": 1563800901213,
            "invalidatedAt": 9223372036854775807
          },
          "action": "DELETE",
          "createdAt": 1563800901213,
          "updatedAt": 1563800901213
        }
      }
    }
  ]
}
```

**Note:** Applying the diffs patch array to the same object, will result in the full previous object.
