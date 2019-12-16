## Space configuration

!!! Note "Your account needs access to the XYZ Pro Services."

***

This connector provides the user with the possibility to track changes to his space. By activating this feature, every modification of features (insert/update/delete by the **ModifyFeaturesEvent**) is tracked and stored in a separate space.

To activate it, just add the listener to your space:

``` javascript
{
  "title": "Test space to track changes.",
  "listeners": { 
    "activity-log": [{
      "params": {
        "states": 3,            //(Optional) Keeps a maximum of x states per object (x > 0).
        "storageMode": "DIFF_ONLY" | "FULL" | ("FEATURE_ONLY" or <none> -> default)
      }
    }]
  }
}
```

The storage mode decides how the features will be stored.

* FEATURE_ONLY: Will store features with some history relative properties (defined below).
* DIFF_ONLY: Will store features with a 'diff'.'ops' property in the XYZ Activity-Log namespace, containing the RFC-6902 diff to its previous object. The features after the HEAD will only contain the XYZ Activity-Log & XYZ namespace properties.
* FULL: Will store features with some history relative properties and a 'diff'.'ops' property in the XYZ Activity-Log namespace, containing the RFC-6902 diff to its previous object.

**ATTENTION**: Applying the diff to the current feature will return the previous (older) feature. This means that adding a new property to a feature, will be shown as 'remove' & 'pathToNewProperty' in the diff of the current.

**IMPORTANT**: Since it is technically not possible to write changes for *<span style="color:red">DeleteFeaturesByTagEvent</span>*, this event type is completely forbidden and the support of this event in the current space is removed.

Within the ModifySpaceEvent, the listener prepares everything and modifies the space to log changes. It will create a new space where the features will be written to, register a new listener that will actually write the modifications and add a processor to prohibit the DeleteByTagEvents.  
Since the ModifyFeatureEvent.response is picked up in a listener, the actual features to log are written asynchronously to the newly created space. As a result, the performance of the actual request is not decreased.

A full space definition with this feature enabled looks like this:

``` JSON
{
    "id": "<yourSpaceId>",
    "title": "Test space to track changes.",
    "description": null,
    
    "enableUUID": true,
    "listeners": {
        "activity-log": [{
            "params": {
                "states": 5,
                "storageMode": "DIFF_ONLY"
            },
            "eventTypes": [
                "ModifySpaceEvent.request"
            ]
        }],
        "activity-log-writer": [{
            "params": {
                "spaceId": "<someNewlyCreatedSpaceId>",
                "states": 5,
                "storageMode": "DIFF_ONLY",
            "spaceType": "MAIN"},
            "eventTypes": [
                "ModifyFeaturesEvent.response",
            "ModifySpaceEvent.request"]
        }]
    },
    "processors":{
        "activity-log-writer": [{
            "id": "activity-log-writer",
            "eventTypes": [
                "DeleteFeaturesByTagEvent.request"
            ]
        }]
    }
}
```

## Written features

***
The features written to the new space are stored by uuid. So the space with this feature enabled needs to have **enableUUID** set to true.

Additionally, the original feature element will be slightly modified:

* The id of the feature will be the **uuid** of the modified feature in the original space.
* The original properties of **'@ns:com:here:xyz'** will be stored under '@ns:com:here:xyz:log'.**original**
* The original id of the feature will be stored under '@ns:com:here:xyz:log'.**id**
* A new property will be added under '@ns:com:here:xyz:log'.**invalidatedAt** to indicate the time range, when this object was the HEAD (newest) history object.
* If diff calculation is enabled, an array with differences in accord with RFC-6902 (JSON Patch) will be added under '@ns:com:here:xyz:log'.**diff**.**ops**

    * **ATTENTION**: Applying the diff to the current feature, will return the previous (older) feature. This means that adding a new property to a feature, will be shown as 'remove' & 'pathToNewProperty' in the diff of the current.

* The action of the operation (SAVE/UPDATE/DELETE) will be stored under '@ns:com:here:xyz:log'.**action**
* Additional entries will be added to the tags, like:
  * Tracking tags defined by the user (userId, changeId, streamId) //TO BE DONE

After you added a new feature to a space, as follows:

GET {space}/features/1 from original space:

``` JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "1",
      "type": "Feature",
      "properties": {
        "version": 0,
        "@ns:com:here:xyz": {
          "uuid": "00000000000000000000000000001",
          "space": "mGGzwMoi",
          "createdAt": 1563451570741,
          "updatedAt": 1570799106542
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

... will look like this in the new space:

Modifications after creation:

* The property 'version' was updated
* The property 'version' was updated and a new sub property was added
* The complete feature was deleted.

Depending on the storage mode, the features may or may not contain diffs, in this case FULL.)

``` JSON
{
  "type": "FeatureCollection",
  "features": [
    {
      "id": "00000000000000000000000000002",
      "type": "Feature",
      "properties": {
        "version": 1,
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "<someNewlyCreatedSpaceId>",
          "createdAt": 1570802212642,
          "updatedAt": 1570802223425
        },
        "@ns:com:here:xyz:log": {
          "id": "1",
          "diff": {
            "add": 0,
            "ops": [
              {
                "op": "replace",
                "path": "/properties/version",
                "value": 0
              }
            ],
            "copy": 0,
            "move": 0,
            "remove": 0,
            "replace": 1
          },
          "action": "UPDATE",
          "original": {
            "puuid": "00000000000000000000000000001",
            "space": "<yourSpaceId>",
            "createdAt": 1563451570741,
            "updatedAt": 1570802185382
          },
          "invalidatedAt": 1570802185392
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
      "id": "00000000000000000000000000003",
      "type": "Feature",
      "properties": {
        "version": 2,
        "newSubObject": {
          "foo":"bar"
        },
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "<someNewlyCreatedSpaceId>",
          "createdAt": 1570802221177,
          "updatedAt": 1570802228766
        },
        "@ns:com:here:xyz:log": {
          "id": "1",
          "diff": {
            "add": 0,
            "ops": [
              {
                "op": "remove",
                "path": "/properties/newSubObject"
              },
              {
                "op": "replace",
                "path": "/properties/version",
                "value": 1
              }
            ],
            "copy": 0,
            "move": 0,
            "remove": 1,
            "replace": 1
          },
          "action": "UPDATE",
          "original": {
            "puuid": "00000000000000000000000000002",
            "space": "<yourSpaceId>",
            "createdAt": 1563451570741,
            "updatedAt": 1570802185392
          },
          "invalidatedAt": 1570802226452
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
      "id": "00000000000000-new-random-uuid",
      "type": "Feature",
      "properties": {
        "@ns:com:here:xyz": {
          "tags": [],
          "space": "<someNewlyCreatedSpaceId>",
          "createdAt": 1570802226548,
          "updatedAt": 1570802226548
        },
        "@ns:com:here:xyz:log": {
          "id": "1",
          "action": "DELETE",
          "original": {
            "updatedAt": 1570802226452
          },
          "invalidatedAt": 9223372036854776000
        }
      },
      "geometry": null
    }
  ]
}
```

**Note:** Applying the diffs patch array to the same object, will result in the previous object.
