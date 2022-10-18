---
title: Automate Zoho WorkDrive Folder Creation
image: assets/images/workdrive-icon.png
description: Zoho Deluge function to create a folder inside Zoho Workdrive.
categories:
- Zoho Workdrive
layout: post
keywords:
- Automation
- Zoho
- Teams
- Zoho Deluge
- Folder
- File management
- Low code
- Zoho WorkDrive

---
Zoho WorkDrive application is the online file management for teams from Zoho suite that allows you to store, share your files and much more.

We will learn today a small script to be able to automatically generate a new folder in Workdrive.

Here are some use cases you can wish to use this kind of script:

1. _Create a folder in Zoho WorkDrive by Deal from your CRM to store and share all the important files from the deal._
2. _Create a folder by delivery_

   _(for example if you are Importer you can store all your documents by shipment and be able to access them easily from any third application)_
3. Create a folder by production

   _and much more..._

You can imagine any file management / administrative system you want and automate it.

**Okay ... Let's do some coding!**

### Our Zoho Deluge Script!

In our example, we want to create folders inside a parent Team Folder. Each folder we create should be created by year of creation and be renamed with a specific description as well as the date of creation.

What we need to do:

* **Get the year of our current record date**
* **Define the title of the folder we want to create**

```javascript
//Get the year from our date field
year = input.DATE.getYear();

//Creating our Folder Name 
folderName = "Your Description - "+input.DATE.toString();

//Setting Headers for our Zoho Deluge HTTP request
headers=Map();
headers.put("Accept","application/vnd.api+json");

//HTTP GET request to get the list of the folders already created
//As we are using Zoho Creator, you need to create a connection
//to Zoho Workdrive inside your Zoho Creator account.
folders = invokeurl
[
	url :"https://workdrive.zoho.eu/api/v1/files/{Parent File ID}/files"
	type :GET
	headers:headers
	connection:"zoho_workdrive"
];

foldersList = folders.get("data");
```

* **Check if we have already a folder for that year.**

1. _If yes, create our new folder inside that folder_
2. _If not, create a folder for that year and create our new folder inside it_

```javascript
nbOfFoldersThisYear = 0;
for each  el in foldersList
{
	if(el.get("attributes").get("name") == year.toString())
	{
		nbOfFoldersThisYear = nbOfFoldersThisYear + 1;
      	currentYearFolderId = el.get("id");
	}
}

if(nbOfFoldersThisYear == 0)
{
	newYearFolder = zoho.workdrive.createFolder(year.toString(),"{Parent File ID}","zoho_workdrive");
	newYearFolderId = newYearFolderId.get("data").get("id");
	newFolder = zoho.workdrive.createFolder(folderName,newYearFolderId,"zoho_workdrive");
}
else
{
	newFolder = zoho.workdrive.createFolder(folderName,currentYearFolderId,"zoho_workdrive");
}
```

* **Use the HTTP response to get the URL of the folder and create a direct link in Zoho Creator Url field type.**

```javascript
newFolderURL = newFolder.get("data").get("attributes").get("permalink");
newFolderURLTitle = "Reports folder";
input.Url = "<a href='" + newFolderURL + "' target='_blank' title='" + newFolderURL 
  			+ "'>" + newFolderURLTitle + "</a>";
```

#### Additional Resources:

[Zoho WorkDrive API](https://workdrive.zoho.com/apidocs/v1/overview "Zoho WorkDrive API")

[Zoho Creator Connections](https://www.zoho.com/creator/newhelp/account-setup/understand-connections.html "Zoho Creator Connections")

[Zoho Creator URL Field](https://help.zoho.com/portal/en/kb/creator/developer-guide/forms/add-and-manage-fields/articles/fields-url-understand#Examples "Zoho Creator URL Field")