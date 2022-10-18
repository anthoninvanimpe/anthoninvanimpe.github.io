---
title: JSON POST Request with Zoho Deluge
description: Learn how to make a HTTP POST request correctly with Zoho Deluge.
tags:
- Zoho
- Zoho Deluge
layout: post
keywords:
- JSON
- API
- Business automation
- Zoho Deluge
- Zoho
- HTTP
categories:
- JSON
- HTTP Request
- Zoho Deluge
image: assets/images/deluge.svg

---
Let's see today how to make a true POST HTTP request with Zoho Deluge!

It can seems really easy to make a post request when you use Javascript (with Fetch or Axios), Python (with Request package) or all other coding languages. 

However, when we tried for the first time to make a POST API call with Zoho Deluge in ZohoCreator, we met some difficulties.

So to be sure that you don't forget your headers for your POST HTTP Request with an JSON object, we show you today a small function that can help you to post information to any third application API!

###  What we need to do?

* _Get the information from our Deal in Zoho CRM_
* _Generate a Deluge Map object with the information we want to pass in the payload and **be sure it is passed as a string**_
* _Set the headers for our HTTP Request_
  1. _Make sure to set `"Content-Type" : "application/json"_`
  2. _Make sure to set `"Charset":"UTF-8"_`
* _Send the request and display the response in Zoho Creator console_ 

### Our Zoho Deluge Script

```javascript
void postRequestInJson(Int dealId,String authToken){

dealRecord = zoho.crm.getRecordById("Deals", dealId);

params = Map();
params.put("auth_token",authToken);
params.put("project_name", dealRecord.get("Deal_Name"));

headerMap = Map();
headerMap.put("Content-Type","application/json");
headerMap.put("charset","UTF-8");

response = invokeurl
[
	url : {URL_to_call}
	type : POST
	parameters: toString(params)
	headers: headerMap
];  

info response;

}
```

### Additional Resources:

[Original solution from Zoho Community](https://help.zoho.com/portal/en/community/topic/dose-zoho-creator-support-a-true-json-post "JSON HTTP POST Request")

[InvokeURL Function](https://www.zoho.com/deluge/help/web-data/invokeurl-function.html "Zoho Deluge InvokeURL function")