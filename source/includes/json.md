
# JSON API

The JSON API gives access to Royal Mail PAF data and Geocoding. It supports [CORS](http://www.w3.org/TR/cors/), can return data as JSON or JSONP and allows sorting.

JSON is the preferred way to go for any new custom integrations. Before you do lots of work, please check our list of [Address Finder Plugins](https://craftyclicks.co.uk/plugins/). You might find something suitable there.

We also have a wrapper for our JSON API on Github:

<span class="externals">
	<span><i class="fa fa-github fa-2x fa-vert-mid"></i> <a href="https://github.com/craftyclicks/CraftyClicks.js" target="_blank">JSON API wrapper on Github</a></span>
</span>


## Full Address (RapidAddress)

```javascript
function getTestData(){
	// Pass parameters via JSON
	var parameters = {
		key: "xxxxx-xxxxx-xxxxx-xxxxx",
		postcode: "aa11aa",
		response: "data_formatted"
	};
	var url = "http://pcls1.craftyclicks.co.uk/json/rapidaddress";
	// or via GET parameters
	// var url = "http://pcls1.craftyclicks.co.uk/json/rapidaddress?key=xxxxx-xxxxx-xxxxx-xxxxx&postcode=aa11aa&response=data_formatted";

	request = new XMLHttpRequest();
	request.open('POST', url, false);
	// Only needed for the JSON parameter pass
	request.setRequestHeader('Content-Type', 'application/json');
	// Wait for change and then either JSON parse response text or throw exception for HTTP error
	request.onreadystatechange = function() {
		if (this.readyState === 4){
			if (this.status >= 200 && this.status < 400){
				// Success!
				data = JSON.parse(this.responseText);
			} else {
				throw 'HTTP Request Error';
			}
		}
	};
	// Send request
	request.send(JSON.stringify(parameters));
	return data;
}

// console.log(getTestData());
```

```python
import urllib2
import urllib
import json

def getTestData():
	url = "https://pcls1.craftyclicks.co.uk/json/"

	url += 'rapidaddress'
	params = {
		'postcode': 'aa11aa',
		'key': 'xxxxx-xxxxx-xxxxx-xxxxx',
		'response': 'data_formatted'
	}

	req = urllib2.Request(url + '?' + urllib.urlencode(params))

	res = urllib2.urlopen(req)
	data = json.loads(res.read())
	return data

#print json.dumps(getTestData(), sort_keys=True, indent=4, separators=(',', ': '))
```

```php
<?php
function getTestData(){
	$data = array(
		"postcode"	=> "aa11aa",
		"key"		=> "xxxxx-xxxxx-xxxxx-xxxxx",
		"response"	=> "data_formatted"
	);
	$data_string = json_encode($data);

	$ch = curl_init('http://pcls1.craftyclicks.co.uk/json/rapidaddress');
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_HTTPHEADER, array(
		'Content-Type: application/json',
		'Content-Length: ' . strlen($data_string))
	);

	$result = curl_exec($ch);
	return $result;
}
// echo '<pre>'.getTestData().'</pre>';
?>
```

### HTTP(S) Request

`GET/POST http://pcls1.craftyclicks.co.uk/json/rapidaddress`

or

`GET/POST https://pcls1.craftyclicks.co.uk/json/rapidaddress`

### Query Parameters

Pass parameters either as GET or POST in JSON.

Parameter		| Default		| Required	| Description
---------		| -------		| --------	| -----------
key				|				| yes		| This is your access token
postcode		|				| yes		| This is the postcode, you request data for
callback		|				| no		| If callback is set, the JSON data will be wrapped as JSONP with the function name from the callback
response		| paf_compact	| no		| Response format is either paf_compact (which is all of the parts) or data_formatted (which pre-formats address lines)
lines			| 2				| no 		| Only active when data_formatted is selected in response. Can be 1, 2 or 3. This is the amount of lines the address data is formatted into.
sort			| none			| no 		| Sorts the results. Options are none, asc, desc.
include_geocode	| false			| no 		| Allows including of the geocode data. Options are true/false. Please note that the usage of this feature within this API endpoint, is free.

<aside class="notice">
We suggest using response=data_formatted in most cases, as it's easier to work with. Use response=paf_compact only if you require all address parts separated out.
</aside>

## - data_formatted response

> The API will respond to the request with the following structure.

```json
{
	"delivery_points":[
		{
			"organisation_name":"THE BAKERY",
			"department_name":"",
			"line_1":"1 HIGH STREET",
			"line_2":"CRAFTY VALLEY",
			"udprn":"12345678"
		},
		{
			"organisation_name":"FILMS R US",
			"department_name":"",
			"line_1":"3 HIGH STREET",
			"line_2":"CRAFTY VALLEY",
			"udprn":"12345679"
		}
	],
	"delivery_point_count":2,
	"postal_county":"POSTAL COUNTY",
	"traditional_county":"TRADITIONAL COUNTY",
	"town":"BIG CITY",
	"postcode":"AA1 1AA"
}
```

### Response

The response contains the following information, per postcode:

Parameter				| Description
---------				| -----------
delivery_points			| Address Parts
delivery_point_count 	| Number of elements in the delivery_points array
postal_county 			| Former-Postal county of the postcode
traditional_county		| Traditional county of the postcode
town					| Town name in uppercase (RM standard)
postcode		 		| Nicely formatted postcode


### delivery_points

Parameter			| Description
---------			| -----------
organisation_name	| Registered organisation at the address.
department_name		| Specific department of said organisation.
line_1				| 
line_2				| 
line_3				| 
udprn 				| Royal Mail unique identifier.

## - paf_compact response

> The API will respond to the request with the following structure.

```json
{
    "thoroughfare_count": 1,
    "thoroughfares": [
        {
            "delivery_points": [
                {
                    "organisation_name": "THE BAKERY",
                    "department_name": "",
                    "po_box_number": "",
                    "building_number": "1",
                    "sub_building_name": "",
                    "building_name": "",
                    "udprn": "12345678"
                },
                {
                    "organisation_name": "FILMS R US",
                    "department_name": "",
                    "po_box_number": "",
                    "building_number": "3",
                    "sub_building_name": "",
                    "building_name": "",
                    "udprn": "12345679"
                }
            ],
            "delivery_point_count": 2,
            "dependent_thoroughfare_name": "",
            "dependent_thoroughfare_descriptor": "",
            "thoroughfare_name": "HIGH",
            "thoroughfare_descriptor": "STREET"
        }
    ],
    "postal_county": "POSTAL COUNTY",
    "traditional_county": "TRADITIONAL COUNTY",
    "dependent_locality": "CRAFTY VALLEY",
    "double_dependent_locality": "",
    "town": "BIG CITY",
    "postcode": "AA1 1AA"
}
```

### Response

The response contains the following information, per postcode:

Parameter			| Description
---------			| -----------
thoroughfares		| 
thoroughfare_count 	| Number of elements in the thoroughfares array
postal_county 		| Former-Postal county of the postcode
traditional_county	| Traditional county of the postcode
town				| Town name in uppercase (RM standard)
postcode 			| Nicely formatted postcode
dependent_locality 	|
double_dependent_locality | 

### thoroughfares

Parameter				| Description
---------				| -----------
delivery_points			| 
delivery_point_count 	| Number of elements in the delivery_points array
dependent_thoroughfare_name 	| 
dependent_thoroughfare_descriptor	| 
thoroughfare_name			| 
thoroughfare_descriptor 	| 


### delivery_points

Parameter			| Description
---------			| -----------
organisation_name	| 
department_name		| 
po_box_numer		|
building_number		| 
sub_building_name	| 
building_name		| 
udprn 				| Royal Mail unique identifier

## Street Level (BasicAddress)

```javascript
function getTestData(){
	// Pass parameters via JSON
	var parameters = {
		key: "xxxxx-xxxxx-xxxxx-xxxxx",
		postcode: "aa11aa",
	};
	var url = "http://pcls1.craftyclicks.co.uk/json/basicaddress";
	// or via GET parameters
	// var url = "http://pcls1.craftyclicks.co.uk/json/basicaddress?key=xxxxx-xxxxx-xxxxx-xxxxx&postcode=aa11aa&response=data_formatted";

	request = new XMLHttpRequest();
	request.open('POST', url, false);
	// Only needed for the JSON parameter pass
	request.setRequestHeader('Content-Type', 'application/json');
	// Wait for change and then either JSON parse response text or throw exception for HTTP error
	request.onreadystatechange = function() {
		if (this.readyState === 4){
			if (this.status >= 200 && this.status < 400){
				// Success!
				data = JSON.parse(this.responseText);
			} else {
				throw 'HTTP Request Error';
			}
		}
	};
	// Send request
	request.send(JSON.stringify(parameters));
	return data;
}

// console.log(getTestData());
```

```python
import urllib2
import urllib
import json

def getTestData():
	url = "https://pcls1.craftyclicks.co.uk/json/"

	url += 'basicaddress'
	params = {
		'postcode': 'aa11aa',
		'key': 'xxxxx-xxxxx-xxxxx-xxxxx',
		'response': 'data_formatted'
	}

	req = urllib2.Request(url + '?' + urllib.urlencode(params))

	res = urllib2.urlopen(req)
	data = json.loads(res.read())
	return data

#print json.dumps(getTestData(), sort_keys=True, indent=4, separators=(',', ': '))
```

```php
<?php
function getTestData(){
	$data = array(
		"postcode"	=> "aa11aa",
		"key"		=> "xxxxx-xxxxx-xxxxx-xxxxx",
		"response"	=> "data_formatted"
	);
	$data_string = json_encode($data);

	$ch = curl_init('http://pcls1.craftyclicks.co.uk/json/basicaddress');
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_HTTPHEADER, array(
		'Content-Type: application/json',
		'Content-Length: ' . strlen($data_string))
	);

	$result = curl_exec($ch);
	return $result;
}
// echo '<pre>'.getTestData().'</pre>';
?>
```

### HTTP(s) Request

`GET/POST http://pcls1.craftyclicks.co.uk/json/basicaddress`

or

`GET/POST https://pcls1.craftyclicks.co.uk/json/basicaddress`

### Query Parameters

Pass parameters either in GET or in JSON.

Parameter		| Default		| Required	| Description
---------		| -------		| --------	| -----------
key				|				| yes		| This is your access token
postcode		|				| yes		| This is the postcode, you request data for
callback		|				| no		| If callback is set, the JSON data will be wrapped as JSONP with the function name from the callback
response		| paf_compact	| no		| Response format is either paf_compact (which is all of the parts) or data_formatted (which pre-formats address lines)
lines			| 2				| no 		| Only used when data_formatted is selected in response. Can be 1, 2 or 3. This is the numer of lines the address data is formatted into
sort			| none			| no 		| Sorts the results. Options are none, asc, desc.
include_geocode	| false			| no 		| Allows including of the geocode data. Options are true/false. Please note that the usage of this feature within this API endpoint, is free.

<aside class="notice">
We suggest using response=data_formatted in most cases, as it's easier to work with. Use response=paf_compact only if you require all address parts separated out.
</aside>

## - data_formatted response

> The API will respond to the request with the following structure.

```json
{
	"thoroughfares": [
		{
			"line_1": "HIGH STREET",
			"line_2": "CRAFTY VALLEY"
		}
	],
	"thoroughfare_count": 1,
	"postal_county": "POSTAL COUNTY",
	"traditional_county": "TRADITIONAL COUNTY",
	"town": "BIG CITY",
	"postcode": "AA1 1AA"
}
```

The response contains the following information, per postcode:

Parameter		| Description
---------		| -----------
thoroughfares	| Address Parts
thoroughfare_count 		| Number of elements in the thoroughfares array
postal_county 			| Former-Postal county of the postcode.
traditional_county		| Traditional county of the postcode.
town			| Town name in upper case. (RM standard)
postcode 		| Nicely formatted postcode

## - paf_compact response

> The API will respond to the request with the following structure.

```json
{
    "thoroughfare_count": 1,
    "thoroughfares": [
        {
            "dependent_thoroughfare_name": "",
            "dependent_thoroughfare_descriptor": "",
            "thoroughfare_name": "HIGH",
            "thoroughfare_descriptor": "STREET"
        }
    ],
    "postal_county": "POSTAL COUNTY",
    "traditional_county": "TRADITIONAL COUNTY",
    "dependent_locality": "CRAFTY VALLEY",
    "double_dependent_locality": "",
    "town": "BIG CITY",
    "postcode": "AA1 1AA"
}
```
### Response
The response contains the following information, per postcode:

Parameter		| Description
---------		| -----------
thoroughfares	| Street Parts
thoroughfare_count 		| Number of elements in the thoroughfares array
postal_county 			| Former-Postal county of the postcode.
traditional_county		| Traditional county of the postcode.
town			| Town name in capital letters. (RM standard)
postcode 		| Formatted Postcode
dependent_locality |
double_dependent_locality | 


### thoroughfares

Parameter		| Description
---------		| -----------
dependent_thoroughfare_name 			| 
dependent_thoroughfare_descriptor		| 
thoroughfare_name			| Name of the thoroughfare
thoroughfare_descriptor 		| Type of the thoroughfare. (Street / Road / Lane / Court etc.)



## Geocoding

```javascript
function getTestData(){
	// Pass parameters via JSON
	var parameters = {
		key: "xxxxx-xxxxx-xxxxx-xxxxx",
		// supply the array of postcodes to geocode. max 25.
		postcodes: ["aa11aa","aa11ab"],
		distance: {
			// supply a postcode to measure to
			"base_postcode": "aa11ae"
			/* or northing easting
			"base_ne" : {
				"northing" : 182949,
				"easting"  : 542267
			}
			*/
		},
		preserve_index: 1
	};
	var url = "http://pcls1.craftyclicks.co.uk/json/geocode";
	// or via GET parameters
	// var url = "http://pcls1.craftyclicks.co.uk/json/geocode?key=xxxxx-xxxxx-xxxxx-xxxxx&postcode=aa11aa";

	request = new XMLHttpRequest();
	request.open('POST', url, false);
	// Only needed for the JSON parameter pass
	request.setRequestHeader('Content-Type', 'application/json');
	// Wait for change and then either JSON parse response text or throw exception for HTTP error
	request.onreadystatechange = function() {
		if (this.readyState === 4){
			if (this.status >= 200 && this.status < 400){
				// Success!
				data = JSON.parse(this.responseText);
			} else {
				throw 'HTTP Request Error';
			}
		}
	};
	// Send request
	request.send(JSON.stringify(parameters));
	return data;
}

// console.log(getTestData());
```

```python
import urllib2
import urllib
import json

def getTestData():
	url = "https://pcls1.craftyclicks.co.uk/json/"

	url += 'basicaddress'
	params = {
		'postcode': 'aa11aa',
		'key': 'xxxxx-xxxxx-xxxxx-xxxxx',
		'response': 'data_formatted'
	}

	req = urllib2.Request(url + '?' + urllib.urlencode(params))

	res = urllib2.urlopen(req)
	data = json.loads(res.read())
	return data

#print json.dumps(getTestData(), sort_keys=True, indent=4, separators=(',', ': '))
```

```php
<?php
function getTestData(){
	$data = array(
		"postcodes" => ["aa11aa","aa11ab"],
		"key" => "xxxxx-xxxxx-xxxxx-xxxxx",
		"distance" => [
			"base_postcode" => "aa11ae"
		]
	);
	$data_string = json_encode($data);
	 
	$ch = curl_init('http://pcls1.craftyclicks.co.uk/json/geocode');
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
	curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_HTTPHEADER, array(
		'Content-Type: application/json',
		'Content-Length: ' . strlen($data_string))
	);

	$result = curl_exec($ch);
	return $result;
}
// echo '<pre>'.getTestData().'</pre>';
?>
```

### HTTP(S) Request

`GET/POST http://pcls1.craftyclicks.co.uk/json/geocode`

or

`GET/POST https://pcls1.craftyclicks.co.uk/json/geocode`

### Query Parameters

Pass parameters either in GET or in JSON. Some parameters are only available in JSON.

Parameter		| Type		| Required	| Available 	| Description
---------		| -------		| --------	| ----------	| -----------
key				| string		| yes		| Both		 	| This is your access token
postcodes		| array of strings	| yes	| JSON 			| This is the postcodes you request data for (Max 25)
postcode		| string		| yes		| GET 			| This is the postcode, you request data for
callback		| string		| no		| Both 			| If callback is set, the JSON data will be wrapped as JSONP with the function name from the callback.
distance		| object		| no		| JSON 			| Specify an extra location to calculate distance to
preserve_index	| bool			| no 		| JSON 			| Specify how the results should be grouped. false: index by cleaned postcode, true: keep input order and return array 

## - response

> The API will respond to the request with the following structure, if preserve_index is unset or false.

```json
{
    "AA11AA": {
        "lat": 51.5132253,
        "lng": -0.0748047,
        "os": {
            "easting": 533689,
            "northing": 181123
        },
        "distance": 46922
    },
    "AA11AB": {
        "lat": 51.513362,
        "lng": -0.0891883,
        "os": {
            "easting": 532691,
            "northing": 181112
        },
        "distance": 45930
    }
}
```
> The API will respond to the request with the following structure, if preserve_index is true.

```json
[
    {
        "lat": 51.5132253,
        "lng": -0.0748047,
        "os": {
            "easting": 533689,
            "northing": 181123
        },
        "distance": 46922,
        "postcode": "AA11AA"
    },
    {
        "lat": 51.513362,
        "lng": -0.0891883,
        "os": {
            "easting": 532691,
            "northing": 181112
        },
        "distance": 45930,
        "postcode": "AA11AB"
    }
]
```

The response contains the following information, per postcode:

Parameter		| Description
---------		| -----------
lat				| Latitude
lng 			| Longitude
os 				| If the postcode exists on OS Code Point Open, northing & easting is available. If the data is not part of the OS dataset, it's not set. In this case, the lat & lng are estimates only.
distance		| In meters. (the official Ordnance Survey NE system is based on the metric system.)
