API Methods
==

_This file is auto-generated using the [api-methods.json](https://github.com/straup/open311-simple/blob/master/api-methods.json) specification and the [mk-api-docs](https://github.com/straup/open311-simple/blob/master/bin/mk-api-docs.py) program and compliments the [general API notes](https://github.com/straup/open311-simple/blob/master/api.md)_.

open311.incidents
==

open311.incidents.getIdFromToken
--

Corresponds to [GET request_id from a token](http://wiki.open311.org/GeoReport_v2#GET_request_id_from_a_token) in GeoReport v2

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **** -  - _Required_

**Notes**

* TODO: port documentation from GeoReport v2

* (seems like this could/should be rolled in to incidents.getInfo, w/ token as an optional parameter) --NG

**Response**


**Possible Errors**


**Example**

	GET http://example.gov/open311/v2/tokens/{token_id}.{format}

	<?xml version="1.0" encoding="utf-8"?>
	<service_requests>
		<request>
			<service_request_id>638344</service_request_id>
			<token>12345</token>
		</request>
	</service_requests>
	
	-- OR --
	
	[
	  {
	    "service_request_id":638344,
	    "token":12345
	  }
	]
open311.incidents.getInfo
--

Corresponds to [GET Service Requests](http://wiki.open311.org/GeoReport_v2#GET_Service_Requests) in GeoReport v2

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **incident\_id** - The unique ID of the incident to get information about. - _Required_

**Notes**

* TODO: port documentation from GeoReport v2

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

Create service requests. Corresponds to [POST Service Request](http://wiki.open311.org/GeoReport_v2#POST_Service_Request) in GeoReport v2

**Method**

[POST](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **jurisdiction\_id** - A valid jurisdiction as defined by the city operating the Open311API - _Required_
* **service\_code** - Obtained from open311.services.getInfo - _Required_
* **location** - Either lat & long or address\_string or address\_id must be submitted - _Required_
* **attribute** - An array of key/value responses based on Service Definitions. This takes the form of attribute[code]=value where multiple code/value pairs can be specified as well as multiple values for the same code in the case of a multivaluelist datatype (attribute[code1][]=value1&attribute[code1][]=value2&attribute[code1][]=value3) - see example.  only required if the service\_code requires a service definition with required fields.
* **lat** - Latitude using the (WGS84) projection.
* **long** - Longitude using the (WGS84) projection.
* **address\_string** - Human entered address or description of location. This should be written from most specific to most general geographic unit, eg address number or cross streets, street name, neighborhood/district, city/town/village, county, postal code. This is required if no lat/lon is provided.
* **address\_id** - The internal address ID used by a jurisdictions master address repository or other addressing system.
* **email** - The email address of the person submitting the request.
* **device\_id** - The unique device ID of the device submitting the request. This is usually only used for mobile devices. For example, Android devices use TelephonyManager.getDeviceId() and iPhone's use [UIDevice currentDevice].uniqueIdentifier
* **account\_id** - The unique ID for the user account of the person submitting the request.
* **first\_name** - The first name of the person submitting the request.
* **last\_name** - The family name of the person submitting the request.
* **phone** - The phone number of the person submitting the request.
* **description** - A full description of the request or report being submitted. This may contain line breaks, but not html or code. Otherwise, this is free form text limited to 4,000 characters.
* **media\_url** - A URL to media associated with the request, eg an image. A convention for parsing media from this URL has yet to be established, so currently it will be done on a case by case basis. For example, if a jurisdiction accepts photos submitted via Twitpic.com, then clients can parse the page at the Twitpic URL for the image given the conventions of Twitpic.com. This could also be a URL to a media RSS feed where the clients can parse for media in a more structured way.

**Notes**

* All dates should be passed in using the [W3C DateTime format](http://www.w3.org/TR/NOTE-datetime) format.

* All geographic data is expected to be using the unprojected [WGS84](http://spatialreference.org/ref/epsg/4326/) datum (read: plain old latitudes and longitudes).

**Response**

* **service\_requests** - Root element.
* **request** - Container for service\_request\_id, token, service\_notice, account\_id.
* **service\_request\_id** - The unique ID of the service request created. This will not be returned if token is returned.
* **token** - If returned, use this to call GET request\_id from a token This will not be returned if service\_request\_id is returned.
* **service\_notice** - Information about the action expected to fulfill the request or otherwise address the information reported. (may not be returned).
* **account\_id** - The unique ID for the user account of the person submitting the request. (may not be returned).

**Possible Errors**

* **404** - service\_code or jurisdiction\_id was not found (specified in error response).
* **400** - service\_code or jurisdiction\_id was not provided (specified in error response).
* **400** - General Service Error (Any failure during create request processing, eg CRM is down. Client will need to notify us).

**Example**

	POST http://example.gov/open311/v2/requests.{format}

	<?xml version="1.0" encoding="utf-8"?>
	<service_requests>
		<request>
			<service_request_id>293944</service_request_id>
			<service_notice>
				The City will inspect and require the responsible party to correct within 24 hours and/or issue a Correction Notice or Notice of Violation of the Public Works Code
			</service_notice>
			<account_id/>
		</request>
	</service_requests>
	
	-- OR --
	
	[
	  {
	    "service_request_id":293944,
	    "service_notice":"The City will inspect and require the responsible party to correct within 24 hours and/or issue a Correction Notice or Notice of Violation of the Public Works Code",
	    "account_id":null
	  }
	]

open311.incidents.search
--

Returns a list of incidents matching a search criteria as defined by the API request.  Corresponds to [GET Service Requests](http://wiki.open311.org/GeoReport_v2#GET_Service_Requests) in GeoReport v2

**Method**

[GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)

**Parameters**

* **** - 
* **page** - The page of results to return. If this argument is omitted, it defaults to 1.
* **per_page** - Number of results to return per page. If this argument is omitted, it defaults to 100. The maximum allowed value is left to the discretion of individual cities.

**Notes**

* TO DO: port from GeoReport v2

* GeoReport v2 does offer some of the functionality described in open311-simple.incidents.search, with the exception of 'where'

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

Defines attributes associated with a service code. These attributes can be unique to the city/jurisdiction. This call is only necessary if the Service selected has metadata set as true from the GET Services response.  Corresponds to [GET Service Definition](http://wiki.open311.org/GeoReport_v2#GET_Service_Definition) in GeoReport v2

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

Provide a list of acceptable 311 service request types and their associated service codes. These request types can be unique to the city/jurisdiction.  Corresponds to [GET Service List](http://wiki.open311.org/GeoReport_v2#GET_Service_List) in GeoReport v2

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

Return a list of valid statuses for incidents. The types of statuses and their meaning are left to the discretion of individual cities.  **NOT represented** in GeoReport v2

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

Returns a list of geographic prefixes that may be used to query for incident reports using the 'open311.incidents.search' API method.  **NOT represented** in GeoReport v2

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

