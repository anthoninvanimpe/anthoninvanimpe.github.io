---
title: Zoho Workdrive
description: Zoho Deluge function to check how many files are in a specific Zoho WorkDrive
  folder.
tags:
- Zoho Deluge
- Zoho Workdrive
layout: post
categories:
- Zoho WorkDrive
author: Zoho Tricks
image: "/assets/images/workdrive-icon.png"

---
Here is an example of an function to check the number of files insides a folder.

```javascript

int Utilities.checkNumberOfFiles(list folderIdsList)
{

  for each  folderId in folderIdsList
  {
    
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
		
    return filesNb;
  }
}
```

#### Additional Resources:

[Zoho WorkDrive API](https://workdrive.zoho.com/apidocs/v1/overview "Zoho WorkDrive API")