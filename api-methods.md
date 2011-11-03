API Methods
==

_This file is auto-generated using the [api-methods.json](https://github.com/straup/open311-simple/blob/master/api-methods.json) specification and the [mk-api-docs](https://github.com/straup/open311-simple/blob/master/bin/mk-api-docs.py) program and compliments the [general API notes](https://github.com/straup/open311-simple/blob/master/api.md)_.

open311.incidents
==

open311.incidents.getInfo
--



**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **incident\_id** - The unique ID of the incident to get information about. - _Required_

**Notes**

* All dates are recorded using the [W3C DateTime format](http://www.w3.org/TR/NOTE-datetime) format.

* All geographic data is returned using the unprojected [WGS84](http://spatialreference.org/ref/epsg/4326/) datum (read: plain old latitudes and longitudes).

**Response**


**Possible Errors**


**Example**

	GET http://example.gov/open311/v2/requests/{id}.{format}

	{
		"incident": {
			"id": 999,
			"service_id": 2,
			"status_id": 1,
			"reported": "..."
		}
	}

open311.incidents.report
--

**This method requires authentication**

Report an incident for a given service. Returns a unique ID for the incident that may be used to call the _open311.incidents.status_ API method.

**Method**

[POST](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **service\_id** - A valid service\_id as defined by the city operating the Open 311 (Simple) API - _Required_
* **latitude** - A valid WGS84 coordinate - _Required_
* **longitude** - A valid WGS84 coordinate - _Required_
* **description** - A free-form text field in which the user reporting the incident may leave additional notes.

**Notes**

* All dates should be passed in using the [W3C DateTime format](http://www.w3.org/TR/NOTE-datetime) format.

* All geographic data is expected to be using the unprojected [WGS84](http://spatialreference.org/ref/epsg/4326/) datum (read: plain old latitudes and longitudes).

**Response**


**Possible Errors**


**Example**

	POST http://example.gov/open311/v2/requests.xml

	{
		"incident": {
			"id": 999,
			"service_id": 2,
			"status_id": 1,
			"reported": "..."
		}
	}

open311.incidents.search
--

Returns a list of incidents matching a search criteria as defined by the API request.

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **service\_id** - The unique ID of the service type to search for. Multiple services may be passed in as a comma-separated list.
* **incident\_id** - The unique ID of the incident to search for. Multiple incidents may be passed in as a comma-separated list.
* **status\_id** - The unique ID of a status type to search for. Multiple statuses may be passed in as a comma-separated list.
* **created** - The date or date range (see [api.md](https://github.com/straup/open311-simple/blob/master/api.md) for details) of when an incident was reported.
* **modified** - The date or date range (see [api.md](https://github.com/straup/open311-simple/blob/master/api.md) for details) of when an incident was last modified.
* **where** - A geopgraphic location or extent (see [api.md]((https://github.com/straup/open311-simple/blob/master/api.md) for details) for details) in which to scope the query.
* **page** - The page of results to return. If this argument is omitted, it defaults to 1.
* **per_page** - Number of results to return per page. If this argument is omitted, it defaults to 100. The maximum allowed value is left to the discretion of individual cities.

**Notes**

* All dates should be passed to the API (and returned in results) using the [W3C DateTime format](http://www.w3.org/TR/NOTE-datetime).

* All geographic data should be passed to the API using the unprojected [WGS84](http://spatialreference.org/ref/epsg/4326/) datum (read: plain old latitudes and longitudes).

* If called with a valid OAuth token and signature then the query will be scoped to the user associated with that token.

* Parameterless searches are not permitted. You must define at least one search criteria.

**Response**


**Possible Errors**


**Example**

	GET http://example.gov/open311/v2/requests.{format}

	{
		"total": 2,
		"per_page": 100,
		"page": 1,
		"incidents": [
			{ "id": 999, "service_id": 2, "status_id": 1, "reported": "..." },
			{ "id": 23, "service_id": 3, "status_id": 1, "reported": "..." },
		]
	}

open311.services
==

open311.services.getInfo
--

Defines attributes associated with a service code. These attributes can be unique to the city/jurisdiction. This call is only necessary if the Service selected has metadata set as true from the GET Services response.

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **service\_code** - A valid service\_code to get information about. - _Required_
* **jurisdiction\_id** - A valid jurisdiction\_id to get information about. - _Required_

**Response**

* **service\_definition** - Root element.
* **service\_code** - Returns the service\_code associated with the definition, the same one submitted for this call.
* **attributes** - Container for attribute.
* **variable** - Options: true, false
    * true denotes that user input is needed.
    * false means the attribute is only used to present information to the user within the description field.
* **code** - A unique identifier for the attribute
* **datatype** - Denotes the type of field used for user input. Options: string, number, datetime, text, singlevaluelist, multivaluelist
    * string: A string of characters without line breaks. Represented in an HTML from using an <input> tag
    * number: A numeric value. Represented in an HTML from using an <input> tag
    * datetime: The input generated must be able to transform into a valid ISO 8601 date. Represented in an HTML from using <input> tags
    * text: A string of characters that may contain line breaks. Represented in an HTML from using an <textarea> tag
    * singlevaluelist: A set of predefined values (specified in this response) where only one value may be selected. Represented in an HTML from using the <select> and <option> tags
    * multivaluelist: A set of predefined values (specified in this response) where several values may be selected. Represented in an HTML from using the <select multiple="multiple"> and <option> tags
* **required** - Setting true means that the value is required to submit service request.  Options: true, false.
* **datatype\_description** - A description of the datatype which helps the user provide their input.
* **order** - The sort order that the attributes will be presented to the user. 1 is shown first.  Options: Any positive integer not used for other attributes in the same service\_code.
* **description** - A description of the attribute field with instructions for the user to find and identify the requested information.
* **values** - A container for a list of predefined options for singlevaluelist or multivaluelist. Required if datatype is set to singlevaluelist or multivaluelist.
* **value** - A container for a predefined option (key and name) for singlevaluelist or multivaluelist.
* **key** - The unique identifier associated with an option for singlevaluelist or multivaluelist. This is analogous to the value attribute in an html option tag.
* **name** - The human readable title of an option for singlevaluelist or multivaluelist. This is analogous to the innerhtml text node of an html option tag.

**Possible Errors**

* **404** - service\_code or jurisdiction\_id provided were not found (specify in error response).
* **400** - service\_code or jurisdiction\_id was not provided (specify in error response).
* **400** - General service error (Anything that fails during service list processing. The client will need to notify us).

**Example**

	GET http://example.gov/open311/v2/services/{id}.{format}

	<?xml version="1.0" encoding="utf-8"?>
	<service_definition>
		<service_code>DMV66</service_code>	
		<attributes>
			<attribute>
				<variable>true</variable>
				<code>WHISHETN</code>
				<datatype>singlevaluelist</datatype>
				<required>true</required>
				<datatype_description></datatype_description>		
				<order>1</order>	
				<description>What is the ticket/tag/DL number?</description>
				<values>
					<value>
						<key>123</key>
						<name>Ford</name>
					</value>
					<value>
						<key>124</key>
						<name>Chrysler</name>
					</value>			
				</values>
			</attribute>	
		</attributes>
	</service_definition>
	
	-- OR --
	
	{
	  "service_code":"DMV66",
	  "attributes":[
	    {
	      "variable":true,
	      "code":"WHISHETN",
	      "datatype":"singlevaluelist",
	      "required":true,
	      "datatype_description":null,
	      "order":1,
	      "description":"What is the ticket/tag/DL number?",
	      "values":[
	        {
	          "key":123,
	          "name":"Ford"
	        },
	        {
	          "key":124,
	          "name":"Chrysler"
	        }
	      ]
	    }
	  ]
	}
open311.services.getList
--

Provide a list of acceptable 311 service request types and their associated service codes. These request types can be unique to the city/jurisdiction

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **jurisdiction\_id** - A valid jurisdiction\_id to get information about. - _Required_
* **page** - The page of results to return. If this argument is omitted, it defaults to 1.
* **per_page** - Number of results to return per page. If this argument is omitted, it defaults to 100. The maximum allowed value is left to the discretion of individual cities.

**Response**

* **services** - Root element.
* **service** - Container for service\_code, service\_name, description, metadata, type, keywords, group.
* **service\_code** - The unique identifier for the service request type.
* **service\_name** - The human readable name of the service request type.
* **description** - A brief description of the service request type.
* **metadata** - Possible values: true, false
    * true: This service request type requires additional metadata so the client will need to make a call to the Service Definition method
    * false: No additional information is required and a call to the Service Definition method is not needed.
* **type** - Possible values: realtime, batch, blackbox
    * realtime: The service request ID will be returned immediately after the service request is submitted.
    * batch: A token will be returned immediately after the service request is submitted. This token can then be later used to return the service request ID.
    * blackbox: No service request ID will be returned after the service request is submitted.
* **keywords** - A comma separated list of tags or keywords to help users identify the request type. This can provide synonyms of the service\_name and group.
* **group** - A category to group this service type within. This provides a way to group several service request types under one category such as "sanitation"

**Possible Errors**

* **404** -  jurisdiction\_id provided was not found (specify in error response).
* **400** - jurisdiction\_id was not provided (specify in error response).
* **400** - General service error (Anything that fails during service list processing. The client will need to notify us).

**Example**

	GET http://example.gov/open311/v2/services.{format}

	<?xml version="1.0" encoding="utf-8"?>
	<services>
		<service>
			<service_code>001</service_code>
			<service_name>Cans left out 24x7</service_name>
			<description>Garbage or recycling cans that have been left out for more than 24 hours after collection. Violators will be cited.</description>
			<metadata>true</metadata>
			<type>realtime</type>
			<keywords>lorem, ipsum, dolor</keywords>
			<group>sanitation</group>
		</service>
	</services>
	
	-- OR --
	
	[
	  {
	    "service_code":001,
	    "service_name":"Cans left out 24x7",
	    "description":"Garbage or recycling cans that have been left out for more than 24 hours after collection. Violators will be cited.",
	    "metadata":true,
	    "type":"realtime",
	    "keywords":"lorem, ipsum, dolor",
	    "group":"sanitation"
	  }
	]

open311.statuses
==

open311.statuses.getList
--

Return a list of valid statuses for incidents. The types of statuses and their meaning are left to the discretion of individual cities.

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **page** - The page of results to return. If this argument is omitted, it defaults to 1.
* **per_page** - Number of results to return per page. If this argument is omitted, it defaults to 100. The maximum allowed value is left to the discretion of individual cities.

**Response**


**Possible Errors**


**Example**

	GET http://example.gov/open311/v2/{foo}

open311.where
==

open311.where.getList
--

Returns a list of geographic prefixes that may be used to query for incident reports using the 'open311.incidents.search' API method.

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **page** - The page of results to return. If this argument is omitted, it defaults to 1.
* **per_page** - Number of results to return per page. If this argument is omitted, it defaults to 100. The maximum allowed value is left to the discretion of individual cities.

**Response**


**Possible Errors**


**Example**

	GET http://example.gov/open311/v2/{foo}

	{
		"total": 3,
		"per_page": 100,
		"page": 1,
		"terms": [
			{ "prefix": "bbox", "description": "...", "example": "bbox:37.788,-122.344,37.857,-122.256" },
			{ "prefix": "near", "description": "...", "example": "near:37.804376,-122.271180" },
			{ "prefix": "zip", "description": "...", "example": "zip:94110" }
		]
	}

