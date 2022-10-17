---
title: 'Zoho CRM'
description: 'Learn how to create a related list in ZohoCRM for ZohoCreator records.'
tags: [Zoho, ZohoCRM, Zoho Deluge, ZohoCreator]
layout: article
---


```javascript
/* First make a connexion in ZohoCRM with ZohoCreator so that you can have access to API
Setup > Developer Space > Connections
More details : https://www.zoho.com/deluge/help/deluge-connections.html#defaultConnection */

//Function to get the records of unpayed invoices from an ZohoCreated Apps and create a related list in the Contact of ZohoCRM
//Parameter 'contactId' should be fetched with the id of the Contact when creating the function in ZohoCRM

//In our ZohoCreator application our invoices are linked to the deals not directly to the Contact
deals = zoho.crm.getRelatedRecords("Deals","Contacts",contactId);

//Setting the variable for the response in XML format for the related list
rowCount = 1;
responseXML = "";

//Creating parameters for the API call with invokeurl deluge built-in funcion
/* More details: 
https://www.zoho.com/crm/developer/docs/functions/migration-to-v2.html#invoke_url
https://www.zoho.com/deluge/help/webhook/invokeurl-api-task.html
https://www.zoho.com/creator/help/api/v2/get-records.html#defining_criteria */

Param = Map();
Param.put("criteria","Status == \"Unpayed\"");
recordsList = invokeurl
[
	url :"https://creator.zoho.eu/api/v2/{App_Owner_Name}/{App_Name}/report/{Report_Name}"
	type :GET
	parameters:Param
	connection:"zoho_creator_reports" // Name of the connection you have set
];

//Get the data from the response in JSON
recordsJson = recordsList.get("data");

//Creating an empty list to add our selected records
invoicesList = List();

//Initiate the XML response
responseXML = responseXML + "<record>";

for each  deal in deals
{
	dealID = deal.get("id");
	for each  el in recordsJson
	{
		invoiceDealID = el.get("Deal_Name); //Getting the id from the integration field in ZohoCreator
    
    //Matching information from ZohoCreator and render each element in a XML format 
		if(invoiceDealID.containsValue(dealID))
		{
			invoiceAmount = el.executeXPath("root/Total_Without_VAT/text()");
			invoiceStatus = el.executeXPath("root/Status/text()");
			invoiceType = el.executeXPath("root/Invoice_Type/text()");
			invoiceDate = el.executeXPath("root/Date_field/text()");
			invoiceDeal = el.executeXPath("root/Deal_Name/ display_value/text()");
			responseXML = responseXML + "<row no='" + rowCount + "'><FL val='Amount without VAT'>" + invoiceAmount + "</FL><FL val='Status'>" + invoiceStatus + "</FL><FL val='Type'>" + invoiceType + "</FL><FL val='Date'>" + invoiceDate + "</FL><FL val='Deal'>" + invoiceDeal + "</FL></row>";
			rowCount = rowCount + 1;
		}
	}
}
responseXML = responseXML + "</record>";
```