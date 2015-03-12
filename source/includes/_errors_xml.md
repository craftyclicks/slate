# Errors

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CraftyResponse>
    <error_code>8001</error_code>
    <error_msg>Invalid or misformatted access token. For details on how to obtain an access token visit www.craftyclicks.co.uk.</error_msg>
</CraftyResponse>
```

The XML API uses the following error codes:

Error Code | Meaning
---------- | -------
0001 | Postcode not found -- The requested postcode doesn't exist.
0002 | Invalid postcode format -- The requested postcode cannot exist. (Misformatted)
1002 | No postcode(s) -- You didn't supply a postcode.
7001 | Demo limit exceeded -- You ran out of free trial clicks.
8001 | Invalid or no access token -- You didn't supply an active access token.
8003 | Account credit allowance exceeded -- You ran out of credits. Please review your balance.
8004 | Access denied due to access rules -- Your token configuration is limiting you.
8005 | Access denied, account suspended -- We disabled your account. Please contact us.
9001 | Internal Server Error -- We had a problem with our server. Try again later.