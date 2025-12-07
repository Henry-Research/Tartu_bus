# Tartu_linnaosad_wgs84.geojson

This is open-source data which was retreived from https://gis.tartulv.ee/arcgis/rest/services/IT/GI_linnaosad/MapServer

## Overview
This GeoJSON file contains the polygon boundaries of **Tartu neighbourhoods (linnaosad)**. It is used to assign each bus stop to a neighbourhood, which serves as a feature in the ML model predicting fare evasion.

---

## Structure
- **type:** `"FeatureCollection"`  
- **name:** `"Tartu_linnaosad_wgs84"`  
- **crs:** CRS84 (WGS84; longitude, latitude)  
- **features:** List of neighbourhood polygons, each with:
  - `"properties"`: `"NIMI"` (neighbourhood name)  
  - `"geometry"`: `"Polygon"` coordinates  


# Official_neighbourhood.jpg

This image simply shows the offical tartu neighborhoods retreived from the academic publication: https://www.researchgate.net/publication/322374300_Network_Connections_and_Neighbourhood_Perception_Using_Social_Media_Postings_to_Capture_Attitudes_among_Twitter_Users_in_Estonia

This was just used to ensure that the Geojson file is an accurate representation of the neighborhoods 


# Neighbourhood_attractivnesspng.jpg

This image was retreived for the above mentioned academic paper, and it represents a feature that was not incorporated into the model but was considered