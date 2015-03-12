# JavaScript Class

Our JavaScript class provides a convenient API for client-side postcode lookup integration into any HTML page.

It is used in most our [Address Finder Plugins](https://craftyclicks.co.uk/plugins/). It's the quickest way to add postcode lookup to your website without having to write much code.

Download the latest version of the JavaScript Class from Github:

<span class="externals">
	<span><i class="fa fa-github fa-2x fa-vert-mid"></i> <a href="https://github.com/craftyclicks/postcode-lookup-js" target="_blank">JavaScript Class on Github</a></span>
</span>

## Creating a lookup object

### Include our core class
```html
<script src="crafty_postcode.class.js"></script>
```

[Download](https://github.com/craftyclicks/postcode-lookup-js/archive/master.zip) a copy of our javascript class and upload crafty_postcode.class.js your server. Include it in your HTML page.

### Instantiate the object

```html
<script>
  // Instantiate the object
  var cp_obj = CraftyPostcodeCreate();
  cp_obj.set("access_token", "xxxxx-xxxxx-xxxxx-xxxxx"); // your token here
  
  cp_obj.set("option_1", "value_1");
    ...
  cp_obj.set("option_n", "value_n");
</script>
```

Instantiate an object and set the config (for detail on all the options, see lower down this page).

### Add multiple lookups

If you want to use address lookup more than once on a page, you can create multiple objects. Each object can be used independently of the others.

```html
<script>
  // Add multiple lookups
  var cp_obj_1 = CraftyPostcodeCreate();
    ....
  var cp_obj_n = CraftyPostcodeCreate();
</script>
```

## Select form elements

There are two methods to tell the object where to find the address form fields:

### Method 1:

Attach unique IDs to all the fields. Set the “form” config to a blank.

```html
<script>
  // Method 1
  cp_obj.set("form", ""); //blank!
  cp_obj.set("elem_company", "companyname_id"); // input field ID
  cp_obj.set("elem_street1", "address1_id"); // input field ID
  //... etc ...
</script>
```

### Method 2:

Tell the script the name of the form and the names of the fields.

```html
<script>
  // Method 2
  cp_obj.set("form", "XYZ"); //form name
  cp_obj.set("elem_company", "companyname"); // input field name
  cp_obj.set("elem_street1", "address1"); // input field name
  //... etc ...
</script>
```

## Minimum configuration

```html
<script>
  var cp_obj = CraftyPostcodeCreate();
  cp_obj.set("access_token", "xxxxx-xxxxx-xxxxx-xxxxx"); // your token here
  cp_obj.set("form", "");
  cp_obj.set("elem_company", "");
  cp_obj.set("elem_street1", "address1");
  cp_obj.set("elem_street2", "");
  cp_obj.set("elem_street3", "");
  cp_obj.set("elem_town", "town");
  cp_obj.set("elem_county", "");
  cp_obj.set("elem_postcode", "postcode");
  cp_obj.set("elem_house_num", ""); 
</script>
```

Here you can find a simple, minimum configuration of our class.

## All Config Options

Option Name     | Description
---------       | -------
access_token    | 
result_elem_id  | 
form            | paf_compact
elem_company    | 2
elem_house_num  | name or id of the input element, can be blank
elem_street1    | name or id of the input element, required, must be set
elem_street2    | name or id of the input element, can be blank
elem_street3    | name or id of the input element, can be blank
elem_town       | name or id of the input element, required, must be set
elem_county     | name or id of the input element, can be blank
elem_postcode   |name or id of the input element, required, must be set; this is both an input and an output
traditional_county | which flavour of county info is shown 0 – postal county (default), 1 – traditional county name
busy_img_url    | set the location of a busy image, default is ‘crafty_postcode_busy.gif’ – this is shown while we wait for the server response
max_width       | set if you want to limit the width of the result box (in px), eg “450px”
max_lines       | set the max number of text lines in the result box
hide_result     | 1 – results box disappears once a result is clicked, 0 – it stays up (default)
org_uppercase   | 0 – company name with leading upper case, 1- all in upper case (default)
town_uppercase  | 0 – leading upper case, 1- all in upper case (default, recommended by Royal Mail)
county_uppercase| 0 – leading upper case (default), 1- all in upper case
addr_uppercase  | 0 – rest of address lines with leading upper case (default), 1- all in upper case
delimiter       | delimiter to use between parts of the address, eg “, ” (default)
msg1            | msg to attach as title to busy image, default – ‘Please wait while we find the address’
err_msg1        | error msg if postcode does not exist, default – ‘This postcode could not be found, please try again or enter your address manually’
err_msg2        | error msg if postcode is not correctly formatted, default – ‘This postcode is not valid, please try again or enter your address manually’
err_msg3        | error msg if there is network problem, default – ‘Unable to connect to address lookup server, please enter your address manually.’
err_msg4        | error msg to cover any other problem, default – (err_num)+’An unexpected error occurred, please enter your address manually.’
res_autoselect  | 1 – the first result will be auto-selected as soon as the result box is shown (default), 0 – it won’t
res_select_on_change | 1 – results will be selected as the user scrolls through the box (default), 0 – user must explicitly click to select
first_res_line  | option to add a dummy 1st line eg ‘—– please select your address —-‘, blank by default
debug_mode      | set it to 1 to get more descriptive error messages in case of trouble, default is 0
lookup_timeout  | maximum time to spend waiting for lookup server response (in milliseconds), default 10000, i.e. 10 seconds.
on_result_ready | callback function name called when result is received from the lookup server
on_result_selected | callback function name called when the user clicks on a result
on_error        | callback function called if there is an error
pre_populate_common_address_parts | 1 – every time the drop-down is shown all common parts of the address get placed on the form, default is 0
lookup_url      | set to use a data relay script, by default set to access the CraftyClicks web service directly
basic_address   | 0 – Rapid/Flexi Address, 1 – Basic Address
single_res_autoselect | 1 – if only one result is found, we select it right away rather than show a one line drop down!
single_res_notice | if single_res_autoselect = 1, show this message if drop down in not shown

## Debugging

If things don’t work right away, set the object into debug mode and see if the error messages give any clues:

```html
<script>
  cp_obj.set("debug_mode",   "1");
</script>
 ```
If you still have no luck – email any error codes/messages to us. We will be happy to help.