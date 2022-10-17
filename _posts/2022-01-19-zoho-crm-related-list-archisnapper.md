---
title: 'Learn how to create a related list in ZohoCRM for the Archisnapper'
categories: [Zoho, ZohoCRM, Zoho Deluge, Archisnapper]
layout: post
image: 'assets/images/crm.svg'
---



```javascript
//Help docs from Zoho
/*https://help.zoho.com/portal/en/kb/zoho-crm-platform/vertical-applications/developer-guide/build-your-crm/articles/customization-adding-custom-related-lists#Create_Custom_Functions
https://www.zoho.com/crm/help/customization/related-lists-dre.html*/

//Help API doc from Archisnapper
//https://docs.archisnapper.com/docs/getting-started


//Function: String archisnapperReports(Int dealId,String authToken)

//Setting the variable for the response in XML format for the related list
rowCount = 1;
responseXML = "";


//Header information for the HTTP methode call and have it in application/json format
headerMap = Map();
headerMap.put("Content-Type","application/json");
headerMap.put("charset","UTF-8");

//Parameters information for the HTTP methode call
paramMap = Map();
paramMap.put("auth_token",authToken);
paramMap.put("unique_id",dealId);

//URL to call
url = "https://app3.archisnapper.com/api/public/v1/sites.json?auth_token=" + authToken;

//API call
responseJson = invokeurl
[
	url :url
	type :GET
	parameters:paramMap
	headers:headerMap
];

// Getting the reports data from the Archisnapper project
visits = responseJson.get(0).get("visits");

//Initiate the XML response
if(visits.isEmpty())
{
	responseXML = responseXML + "<error><message>No report available</message></error>";
}
else
{
	responseXML = responseXML + "<record>";
	for each  rec in visits
	{
		reportTitle = rec.get("title");
		reportStatus = rec.get("status");
		reportDate = rec.get("date");
		reportPDF = rec.get("pdf_link");
		responseXML = responseXML + "<row no='" + rowCount + "'><FL val='Title'>" + reportTitle + "</FL><FL val='Status'>" + reportStatus + "</FL><FL val='Date'>" + reportDate.getDate() + "</FL><FL val='Report PDF' link='true' url='" + reportPDF + "&amp;auth_token=" + authToken + "'>PDF Link</FL></row>";
		rowCount = rowCount + 1;
	}
	responseXML = responseXML + "</record>";
}
return responseXML;
```