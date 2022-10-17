---
title: 'Zoho Deluge'
description: 'Learn to make a http POST request correctly with Zoho Deluge'
tags: [Zoho, Zoho Deluge]
layout: article
---


```javascript
//Solution : https://help.zoho.com/portal/en/community/topic/dose-zoho-creator-support-a-true-json-post

//postRequestInJson(Int dealId,String authToken)

deal_record = zoho.crm.getRecordById("Deals", dealId);

params = Map();
params.put("auth_token",authToken);
params.put("project_name", deal_record.get("Deal_Name"));

headerMap = Map();
headerMap.put("Content-Type","application/json");
headerMap.put("charset","UTF-8");

response = invokeurl
[
	url : <url>
	type :POST
	parameters:toString(params)
	headers:headerMap
]; 
info response;
```