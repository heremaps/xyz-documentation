# USGS Earthquake Data on Hub

The [US Geological Survey](https://www.usgs.gov/) offers a variety of monitored data (volancoes, fires, landslides, etc). For the Hub we implemented realtime data from the [USGS earthquake API](https://earthquake.usgs.gov/fdsnws/event/1/) as a shared space `ID: OgCV7ETt`. The API offers earthquakes from the last 30 days by default and allows to narrow down the search by filtering for magnitude, significance, depth or a specific date.

![Earthquakes in Viewer](../../assets/images/earthquake_example.PNG)

| Shared Space ID | Source | Example |
| ---- | ----- | ----- |
| OgCV7ETt | https://earthquake.usgs.gov/fdsnws/event/1/ | https://studio.here.com/viewer/?project_id=bd0d680d-3451-449c-a850-526b262a19a8 |

### Example FeatureCollection
```
{
	"type": "FeatureCollection",
	"features": [
		{
			"type": "Feature",
			"properties": {
				"place": "8 km SSE of Shiroi, Japan",
				"time": 1603868312036,
				"tz": null,
				"url": "https://earthquake.usgs.gov/earthquakes/eventpage/us7000c6n5",
				"detail": "https://earthquake.usgs.gov/fdsnws/event/1/query?eventid=us7000c6n5&format=geojson",
				"felt": 12,
				"cdi": 3.8,
				"mmi": null,
				"alert": null,
				"status": "reviewed",
				"tsunami": 0,
				"net": "us",
				"code": "7000c6n5",
				"ids": ",us7000c6n5,",
				"sources": ",us,",
				"types": ",dyfi,origin,phase-data,",
				"nst": null,
				"dmin": 1.735,
				"rms": 0.75,
				"gap": 137,
				"magType": "mb",
				"type": "earthquake",
				"title": "M 4.8 - 8 km SSE of Shiroi, Japan",
				"depth": 70.26,
				"magnitude": 4.8,
				"significance": 359,
				"date": "2020-10-28T06:58:32.036Z",
				"@ns:com:here:xyz": {
					"space": "OgCV7ETt",
					"tags": [],
					"updatedAt": 1604038581435
				}
			},
			"geometry": {
				"type": "Point",
				"coordinates": [
					140.1095,
					35.7356
				]
			},
			"id": "us7000c6n5"
		}
	]
}
```

#### Notes
Does not support 
- DeleteFeatures
- LoadFeatures
- ModifyFeatures
- GetFeaturesByGeometry (POST)
