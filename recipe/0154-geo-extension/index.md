---
title: Represent Manifest on a Map with navPlace Extension
id: 154
layout: recipe
tags: [maps, geolocate, annotation]
summary: "Use the navPlace extension to provide geolocation information about an IIIF Presentation API 3.0 Manifest."
---

### Use Case 
A Manifest contains information relating to one or multiple geographic areas. You would like to associate this Manifest with geographic coordinates for use in web mapping clients like Leaflet and OpenLayers. This could mean simply showing a non-interactive shape on a web map, but often more data from the resource is displayed in connection with the shape as a result of available functionality. The example below shows a pop-up that appears upon clicking the shape. The pop-up includes the targeted map as well as other metadata from the resource.

<div id="bigImage">
	<h4 style="color:white;"> Click Image to Close </h4>
	<img onclick="hideBigImage()" style="max-height: 100%; max-width: 100%;" src="./images/leaflet_example.png" />
</div>

### Implementation Notes
You will notice multiple contexts used in the top level `@context` property.  The third party [GeoJSON-LD](https://geojson.org/geojson-ld/) context is included along with the IIIF Presentation API 3 navPlace extension and the IIIF Presentation API 3.0 context. This supplies the vocabulary terms for the GeoJSON-LD used in the value for `navPlace` and the `navPlace` term itself since the IIIF Presentation API 3.0 context does not describe those terms. When there are multiple contexts, the `@context` property can be an array which is processed as a set. Typically order does not matter for a set. However, when the IIIF context is used in these arrays it must be the last item in the set.

GeoJSON `properties` is a generic field and [can be nearly anything](https://tools.ietf.org/html/rfc7946#section-3.2). If, for example, the targeted resource has a `label` and the `properties` field contains a `label`, the consuming interface must make a choice on which to prioritize for presentation purposes. In the example renderings, the label inside `properties` is used as opposed to the label from the Manifest.  

Note that [`geometry` can be more than just a `Point`.](https://tools.ietf.org/html/rfc7946#section-3.1)

### Restrictions
Applications that strictly follow Linked Data practices will find that nested GeoJSON coordinate arrays are incompatible with the processing model of JSON-LD 1.0. The JSON-LD 1.1 processing model does not have this restriction. Be aware if you plan to serialize JSON-LD into [other semantic data formats or markup languages](https://www.w3.org/TR/json-ld11/#relationship-to-other-linked-data-formats) such as RDF.

### Example
`navPlace` contains GeoJSON-LD, which is supported by a number of open source mapping systems. A client can parse `navPlace` from a Manifest and pass the GeoJSON into a web map resulting in rendered geometric shapes on a world map. Often, data from the resource such as an image URL, label or description is connected with those shapes via [`properties`](https://tools.ietf.org/html/rfc7946#section-3.2) in GeoJSON.

{% include manifest_links.html viewers="" manifest="manifest.json" %}

{% include jsonviewer.html src="manifest.json" config='data-line="61-99"' %}

## Related Recipes
* [Represent Canvas Fragment as a Geographic Area in a Web Mapping Client][0139]

{% include acronyms.md %}
{% include links.md %}

<style>
	#bigImage{
		position: fixed;
		top: 0;
		left : 0;
		height : 100em;
		width: 100%;
		background-color: rgba(0,0,0,.8);
		display:none;
		text-align: center;
		padding-top: 4px;
	}
	img{
		cursor: pointer;
	}
	.imagelink{
		margin-right: 1%;
	    display: inline-block;
    	text-decoration: none !important;
    	border-bottom: none !important;
	}
	.imagelink:focus{
    	outline: none !important;
	}
</style>

<script type="text/javascript">
	function showBigImage(){
		document.getElementById("bigImage").style.display = "block"
	}
	function hideBigImage(){
		document.getElementById("bigImage").style.display = "none"
	}
</script>