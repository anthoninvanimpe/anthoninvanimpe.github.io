---
title: Zoho Workdrive, Check How Many Files You Have!
description: ZohoCreator function to check how many files are in a specific folder.
tags:
- Zoho
- ZohoCreator
- Zoho Deluge
- Zoho Workdrive
layout: post
categories:
- Zoho WorkDrive
image: assets/images/workdrive-icon.png
keywords:
- Cloud
- File management
- Low code
- Zoho Deluge
- Zoho Creator
- Zoho WorkDrive

---


### What We Need To Do

* 
   
### Our Zoho Deluge Script

```javascript
void Utilities.checkNumberOfFiles()
{
   upcomingDeals = DEALS[STAGE == "Preparation" && DRIVE != null];
   for each  rec in upcomingDeals
   {
     folderId = rec.DRIVE.getSuffix("https://workdrive.zoho.eu/folder/")\
		.getPrefix(" ").replaceAll("['\"]","");
    
     header = Map();
     header.put("Accept","application/vnd.api+json");
    
     folder = invokeurl
     [
	url :"https://www.zohoapis.eu/workdrive/api/v1/files/" + folderId
	type :GET
	headers:header
	connection:"zoho_workdrive"
      ];
    
     filesNb = folder.get("data").get("attributes")\
		.get("storage_info").get("files_count");
			
     if(filesNb > 0)
     {
	newUrl = rec.DRIVE.replaceAll(" - \( [0-9] file\(s\) \)","")
		.replaceAll("</a>"," - ( " + filesNb + " file(s) )</a>");
		
	rec.DRIVE=newUrl;
      }
   }
}
```

### Additional Resources

[]( "")
