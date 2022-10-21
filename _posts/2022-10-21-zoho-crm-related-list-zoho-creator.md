---
title: Custom Zoho CRM Related List With Zoho Creator Records
description: Learn how to create a related list in ZohoCRM for ZohoCreator records.
tags:
- Zoho
- ZohoCRM
- Zoho Deluge
- ZohoCreator
layout: post
keywords:
- Data
- XML
- Low code
- Zoho Deluge
- Zoho Creator
- Zoho CRM
categories:
- Zoho Creator
- Zoho CRM
image: assets/images/crm.svg

---
Usually, we have a lot of information and data in third apps. It often difficult to keep tracks on those because we need to go from an app to another.

Today, we will show you how to create a related list in Zoho CRM from Zoho Creator app.

This will give your Sales department a quick visibility on specific info.

In our example, we have an app in Zoho Creator to manage the invoicing to clients. We want to fetch all the unpaid invoices in the Contact form.

### What We Need To Do

* Make a connection in ZohoCRM with ZohoCreator so that you can have access to the API .

            _Go to Setup > Developer Space > Connections (_[_More details_](https://www.zoho.com/deluge/help/deluge-connections.html#defaultConnection "Zoho Connections")_)_


* Create a function in Zoho CRM to be used as a related list

            _Go to Setup > Developer Space > Function_
* Get the records of unpaid invoices from an Zoho Creator app
* Create a related list in the Contact view of Zoho CRM by formatting information in XML format

### Our Zoho Deluge Script

```javascript
//Parameter 'contactId' should be fetched with the id of the Contact when creating the function in ZohoCRM

//In our ZohoCreator application our invoices are linked to the deals not directly to the Contact
deals = zoho.crm.getRelatedRecords("Deals","Contacts",contactId);

//Setting the variable for the response in XML format for the related list
rowCount = 1;
responseXML = "";

//Creating parameters for the API call with invokeurl deluge built-in funcionn
Param = Map();
Param.put("criteria","Status == \"Unpaid\"");

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
        //Getting the id from the integration field in ZohoCreator
		invoiceDealID = el.get("Deal_Name");
    
        //Matching information from ZohoCreator 
      	// and render each element in a XML format 
		if(invoiceDealID.containsValue(dealID))
		{
			invoiceAmount = el.executeXPath("root/Total_Without_VAT/text()");
			invoiceStatus = el.executeXPath("root/Status/text()");
			invoiceType = el.executeXPath("root/Invoice_Type/text()");
			invoiceDate = el.executeXPath("root/Date_field/text()");
			invoiceDeal = el.executeXPath("root/Deal_Name/ display_value/text()");
          
			responseXML = responseXML + "<row no='" + rowCount + "'><FL val='Amount without VAT'>"; 
            responseXML += invoiceAmount + "</FL><FL val='Status'>" + invoiceStatus;
            responseXML += "</FL><FL val='Type'>" + invoiceType + "</FL><FL val='Date'>" ;
            responseXML += invoiceDate + "</FL><FL val='Deal'>" + invoiceDeal + "</FL></row>";
          
			rowCount = rowCount + 1;
		}
	}
}
responseXML = responseXML + "</record>";
```

### Additional Resources

[Zoho Connections](https://www.zoho.com/deluge/help/deluge-connections.html#defaultConnection "Zoho Connections")

[Zoho Deluge InvokeURL](https://www.zoho.com/deluge/help/webhook/invokeurl-api-task.html "Zoho Deluge InvokeURL")

[Zoho Creator API Defining Criteria](https://www.zoho.com/creator/help/api/v2/get-records.html#defining_criteria "Zoho Creator API Defining Criteria")