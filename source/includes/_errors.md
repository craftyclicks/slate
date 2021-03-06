# Errors

> JSON error format

```json
{
    "error_code": "0001",
    "error_msg": "No data was found for the requested postcode."
}
```

> XML error format

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CraftyResponse>
    <error_code>0001</error_code>
    <error_msg>No data was found for the requested postcode.</error_msg>
</CraftyResponse>
```

The JSON API uses the following error codes:

Error Code | Meaning
---------- | -------
0001 | Postcode not found -- The requested postcode doesn't exist.
0002 | Invalid postcode format -- The requested postcode cannot exist. (Misformatted)
0006 | Too many postcodes requested -- (Mass Geocoding API, max 25)
1001 | JSON syntax error
1002 | No postcode(s) -- You didn't supply a postcode.
7001 | Demo limit exceeded -- You ran out of free trial clicks.
8001 | Invalid or no access token -- You didn't supply an active access token.
8003 | Account credit allowance exceeded -- You ran out of credits. Please review your balance.
8004 | Access denied due to access rules -- Your token configuration is limiting you.
8005 | Access denied, account suspended -- We disabled your account. Please contact us.
9001 | Internal Server Error -- We had a problem with our server. Try again later.
