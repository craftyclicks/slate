# XML API

> Examples for the XML API are only available in <a href="/?php#xml-api">PHP</a> and <a href="/?python#xml-api">Python</a>.

The XML API gives access to Royal Mail PAF data only (Geocoding is not available). It supports [CORS](http://www.w3.org/TR/cors/).

XML is useful for implementing server side postcode lookup on systems where [JSON](#json-api) is hard to use. Please also check our list of [Address Finder Plugins](https://craftyclicks.co.uk/plugins/). You might find something suitable there and save yourself some integration work.

## Full Address (RapidAddress)

```python
import urllib2
import urllib
import xml.dom.minidom

def getTestData():
    url = "https://pcls1.craftyclicks.co.uk/xml/"

    url += 'rapidaddress'
    params = {
        'postcode': 'aa11aa',
        'key': 'xxxxx-xxxxx-xxxxx-xxxxx',
        'response': 'data_formatted'
    }

    req = urllib2.Request(url + '?' + urllib.urlencode(params))
    res = urllib2.urlopen(req)

    return xml.dom.minidom.parseString( res.read() )

xmldata = getTestData()
print xmldata.toprettyxml()
```

```php
<?php
function getTestData(){
    $data = array(
        "postcode"  => "aa11aa",
        "key"       => "xxxxx-xxxxx-xxxxx-xxxxx",
        "response"  => "data_formatted"
    );

    $ch = curl_init('http://pcls1.craftyclicks.co.uk/xml/rapidaddress?key='.$data["key"].'&postcode='.$data["postcode"].'&response='.$data["response"]);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $result = curl_exec($ch);
    return $result;
}
// echo '<pre>'.getTestData().'</pre>';
?>
```

### HTTP(S) Request

`GET http://pcls1.craftyclicks.co.uk/xml/rapidaddress`

or

`GET https://pcls1.craftyclicks.co.uk/xml/rapidaddress`

### Query Parameters

Pass parameters in GET.

Parameter       | Default       | Required  | Description
---------       | -------       | --------  | -----------
key             |               | yes       | This is your access token
postcode        |               | yes       | This is the postcode, you request data for
response        | paf_compact   | no        | Response format is either paf_compact (which is all of the parts) or data_formatted (which pre-formats address lines)
lines           | 2             | no        | Only used if response=data_formatted. Can be 1, 2 or 3. This is the amount of lines the address data is formatted into.

<aside class="notice">
We suggest using response=data_formatted in most cases, as it's easier to work with. Use response=paf_compact only if you require all address parts separated out.
</aside>

## - data_formatted response

> The API will respond to the request with the following structure.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CraftyResponse>
    <address_data_formatted>
        <delivery_point>
            <organisation_name>THE BAKERY</organisation_name>
            <department_name></department_name>
            <line_1>1 HIGH STREET</line_1>
            <line_2>CRAFTY VALLEY</line_2>
            <udprn>12345678</udprn>
        </delivery_point>
        <delivery_point>
            <organisation_name>FILMS R US</organisation_name>
            <department_name></department_name>
            <line_1>3 HIGH STREET</line_1>
            <line_2>CRAFTY VALLEY</line_2>
            <udprn>12345679</udprn>
        </delivery_point>
        <delivery_point_count>5</delivery_point_count>
        <town>BIG CITY</town>
        <postal_county>POSTAL COUNTY</postal_county>
        <traditional_county>TRADITIONAL COUNTY</traditional_county>
        <postcode>AA1 1AA</postcode>
    </address_data_formatted>
</CraftyResponse>
```

### Response

The response contains the following information, per postcode:

Parameter       | Description
---------       | -----------
delivery_points | Address Parts
delivery_point_count        | Number of elements in the delivery_point array
postal_county           | Former-Postal county of the postcode.
traditional_county      | Traditional county of the postcode.
town            | Town name in upper case (RM standard)
postcode        | Nicely formatted Postcode


### delivery_point

Parameter       | Description
---------       | -----------
organisation_name   | Registered organisation at the address.
department_name     | Specific department of said organisation.
line_1          | 
line_2          | 
line_3          | 
udprn           | Royal Mail unique identifier.

## - paf_compact response

> The API will respond to the request with the following structure.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CraftyResponse>
    <address_data_paf_compact>
        <thoroughfare_count>1</thoroughfare_count>
        <thoroughfare>
            <delivery_point_count>5</delivery_point_count>
            <delivery_point>
                <organisation_name>THE BAKERY</organisation_name>
                <department_name></department_name>
                <po_box_number></po_box_number>
                <building_number>1</building_number>
                <sub_building_name></sub_building_name>
                <building_name></building_name>
                <udprn>12345678</udprn>
            </delivery_point>
            <delivery_point>
                <organisation_name>FILMS R US</organisation_name>
                <department_name></department_name>
                <po_box_number></po_box_number>
                <building_number>3</building_number>
                <sub_building_name></sub_building_name>
                <building_name></building_name>
                <udprn>12345679</udprn>
            </delivery_point>
            <dependent_thoroughfare_name></dependent_thoroughfare_name>
            <dependent_thoroughfare_descriptor></dependent_thoroughfare_descriptor>
            <thoroughfare_name>HIGH</thoroughfare_name>
            <thoroughfare_descriptor>STREET</thoroughfare_descriptor>
        </thoroughfare>
        <double_dependent_locality></double_dependent_locality>
        <dependent_locality>CRAFTY VALLEY</dependent_locality>
        <town>BIG CITY</town>
        <postal_county>POSTAL COUNTY</postal_county>
        <traditional_county>TRADITIONAL COUNTY</traditional_county>
        <postcode>AA1 1AA</postcode>
    </address_data_paf_compact>
</CraftyResponse>
```


### Response

The response contains the following information, per postcode:

Parameter       | Description
---------       | -----------
thoroughfares   | Street Parts
thoroughfare_count      | Number of elements in the thoroughfare array
postal_county           | Former-Postal county of the postcode.
traditional_county      | Traditional county of the postcode.
town            | Town name in capital letters. (RM standard)
postcode        | Formatted Postcode
dependent_locality |
double_dependent_locality | 

### thoroughfare

Parameter       | Description
---------       | -----------
delivery_points | Address Parts
delivery_point_count        | Number of elements in the thoroughfares array
dependent_thoroughfare_name             | 
dependent_thoroughfare_descriptor       | 
thoroughfare_name           | Name of the thoroughfare
thoroughfare_descriptor         | Type of the thoroughfare (Street / Road / Lane / Court etc.)


### delivery_point

Parameter           | Description
---------           | -----------
organisation_name   | Registered organisation at the address.
department_name     | Specific department of said organisation.
po_box_numer        |
building_number     | 
sub_building_name   | 
building_name       | 
udprn               | Royal Mail unique identifier.

## Street Level (BasicAddress)

```python
import urllib2
import urllib
import xml.dom.minidom

def getTestData():
    url = "https://pcls1.craftyclicks.co.uk/xml/"

    url += 'basicaddress'
    params = {
        'postcode': 'aa11aa',
        'key': 'xxxxx-xxxxx-xxxxx-xxxxx',
        'response': 'data_formatted'
    }

    req = urllib2.Request(url + '?' + urllib.urlencode(params))
    res = urllib2.urlopen(req)

    return xml.dom.minidom.parseString( res.read() )

xmldata = getTestData()
print xmldata.toprettyxml()
```

```php
<?php
function getTestData(){
    $data = array(
        "postcode"  => "aa11aa",
        "key"       => "xxxxx-xxxxx-xxxxx-xxxxx",
        "response"  => "data_formatted"
    );

    $ch = curl_init('http://pcls1.craftyclicks.co.uk/xml/basicaddress?key='.$data["key"].'&postcode='.$data["postcode"].'&response='.$data["response"]);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $result = curl_exec($ch);
    return $result;
}
// echo '<pre>'.getTestData().'</pre>';
?>
```

### HTTP(s) Request

`GET http://pcls1.craftyclicks.co.uk/xml/basicaddress`

or

`GET https://pcls1.craftyclicks.co.uk/xml/basicaddress`

### Query Parameters

Pass parameters either in GET or in JSON.

Parameter       | Default       | Required  | Description
---------       | -------       | --------  | -----------
key             |               | yes       | This is your access token
postcode        |               | yes       | This is the postcode, you request data for
response        | paf_compact   | no        | Response format is either paf_compact (which is all of the parts) or data_formatted (which pre-formats address lines)
lines           | 2             | no        | Only used when data_formatted is selected in response. Can be 1, 2 or 3. This is the numer of lines the address data is formatted into.

<aside class="notice">
We suggest using response=data_formatted in most cases, as it's easier to work with. Use response=paf_compact only if you require all address parts separated out.
</aside>

## - data_formatted response

> The API will respond to the request with the following structure.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CraftyResponse>
    <address_data_formatted>
        <thoroughfare>
            <line_1>HIGH STREET</line_1>
            <line_2>CRAFTY VALLEY</line_2>
        </thoroughfare>
        <town>BIG CITY</town>
        <postal_county>POSTAL COUNTY</postal_county>
        <traditional_county>TRADITIONAL COUNTY</traditional_county>
        <postcode>AA1 1AA</postcode>
    </address_data_formatted>
</CraftyResponse>
```

The response contains the following information, per postcode:

Parameter       | Description
---------       | -----------
thoroughfares   | Address Parts
thoroughfare_count      | Number of elements in the thoroughfare array
postal_county           | Former-Postal county of the postcode.
traditional_county      | Traditional county of the postcode.
town            | Town name in upper case (RM standard)
postcode        | Formatted Postcode

## - paf_compact response

> The API will respond to the request with the following structure.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CraftyResponse>
    <address_data_paf_compact>
        <thoroughfare_count>1</thoroughfare_count>
        <thoroughfare>
            <dependent_thoroughfare_name></dependent_thoroughfare_name>
            <dependent_thoroughfare_descriptor></dependent_thoroughfare_descriptor>
            <thoroughfare_name>HIGH</thoroughfare_name>
            <thoroughfare_descriptor>STREET</thoroughfare_descriptor>
        </thoroughfare>
        <double_dependent_locality></double_dependent_locality>
        <dependent_locality>CRAFTY VALLEY</dependent_locality>
        <town>BIG CITY</town>
        <postal_county>POSTAL COUNTY</postal_county>
        <traditional_county>TRADITIONAL COUNTY</traditional_county>
        <postcode>AA1 1AA</postcode>
    </address_data_paf_compact>
</CraftyResponse>
```
### Response
The response contains the following information, per postcode:

Parameter       | Description
---------       | -----------
thoroughfares   | Address Parts
thoroughfare_count      | Number of elements in the thoroughfares array
postal_county           | Former-Postal county of the postcode.
traditional_county      | Traditional county of the postcode.
town            | Town name in capital letters. (RM standard)
postcode        | Formatted Postcode
dependent_locality |
double_dependent_locality | 


### thoroughfare

Parameter       | Description
---------       | -----------
dependent_thoroughfare_name             | 
dependent_thoroughfare_descriptor       | 
thoroughfare_name           | Name of the thoroughfare
thoroughfare_descriptor         | Type of the thoroughfare (Street / Road / Lane / Court etc.)