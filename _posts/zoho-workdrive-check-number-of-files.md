---
title: 'Zoho Workdrive'
description: 'ZohoCreator function to check how many files are in a specific folder'
tags: [Zoho, ZohoCreator, Zoho Deluge, Zoho Workdrive]
layout: article
---

```javascript
void Utilities.checkNumberOfFiles()
{
	upcomingDeals = DEALS[STAGE == "Preparation" && DRIVE != null];
	for each  rec in upcomingDeals
	{
		folderId = rec.DRIVE.getSuffix("https://workdrive.zoho.eu/folder/").getPrefix(" ").replaceAll("['\"]","");
    
		header = Map();
		header.put("Accept","application/vnd.api+json");
    
		folder = invokeurl
		[
			url :"https://www.zohoapis.eu/workdrive/api/v1/files/" + folderId
			type :GET
			headers:header
			connection:"zoho_workdrive"
		];
    
		filesNb = folder.get("data").get("attributes").get("storage_info").get("files_count");
		if(filesNb > 0)
		{
			newUrl = rec.DRIVE.replaceAll(" - \( [0-9] file\(s\) \)","").replaceAll("</a>"," - ( " + filesNb + " file(s) )</a>");
			rec.DRIVE=newUrl;
		}
	}
}
```