---
title: How To Automate Workdrive Folder Creation.
image: assets/images/workdrive-icon.png
description: Zoho Deluge function to create a folder inside Zoho Workdrive.
categories:
- Zoho Workdrive
layout: post

---
### What is Zoho Workdrive?

The Workdrive application is the online file management for teams from Zoho that allows you to store, share your files and much more.

We will learn today a small script to be able to automatically generate a new folder in Workdrive. 

Here are some use cases you can wish to use this kind of script:

1. _Create a folder in Zoho Workdrive by Deal from your CRM to store and share all the important files from the deal._
2. _Create a folder by delivery_ 

   _(for example if you are Importer you can store all your documents by shimpent and be able to access them easily from any third application)_
3. Create a folder by production

    _and much more..._

You can imagine any file management / administrative system you want and automate it.

### Our Zoho Deluge Script!

In our example, we want to create folders inside a parent Team Folder. Each folder we create should be create by year of create and be renamed with a specific description and the date of creation.

What we need to do:

1. **Get the Year of our current record date**
2. **Define the title of the folder we want to create**
3. **Check if we have already a folder for that year.**
   1. _If yes, create our new folder inside that folder_
   2. _If not, create a folder for that year and create our new folder inside it_
4. **Use the HTTP response to get the URL of the folder and create a direct link in Zoho Creator Url field type.**

Okey... **Let's do some coding !**

    zoho.workdrive.createFolder(test_folder_name,year_folder_id,"zoho_workdrive");

```javascript
//Get the year from our date field
year = input.DATE.getYear();

//folder name
folderName = "Your Description - "+input.DATE.toString();

//Setting Headers for our Deluge HTTP request
headers=Map();
headers.put("Accept","application/vnd.api+json");

//HTTP GET request to get the list of the folders already created
folders = invokeurl
[
	url :"https://workdrive.zoho.eu/api/v1/files/{Parent File ID}/files"
	type :GET
	headers:headers
	connection:"zoho_workdrive"
];

foldersList = folders.get("data");
```

```javascript


folders = invokeurl
[
	url :"https://workdrive.zoho.eu/api/v1/files/6jknja843d8a5bfb54cdc9f4445b846f14a0a/files"
	type :GET
	headers:headers
	connection:"zoho_workdrive"
];
folders_list = folders.get("data");
year_folder_nb = 0;
for each  el in folders_list
{
	if(el.get("attributes").get("name") == year.toString())
	{
		year_folder_nb = year_folder_nb + 1;
		year_folder_id = el.get("id");
	}
}
if(year_folder_nb == 0)
{
	new_year_folder = zoho.workdrive.createFolder(year.toString(),"6jknja843d8a5bfb54cdc9f4445b846f14a0a","zoho_workdrive");
	new_year_folder_id = new_year_folder.get("data").get("id");
	test_folder = zoho.workdrive.createFolder(test_folder_name,new_year_folder_id,"zoho_workdrive");
}
else
{
	test_folder = zoho.workdrive.createFolder(test_folder_name,year_folder_id,"zoho_workdrive");
}
test_folder_url = test_folder.get("data").get("attributes").get("permalink");
test_folder_url_title = "Reports folder";
input.Url = "<a href='" + test_folder_url + "' target='_blank' title='" + test_folder_url_title + "'>" + test_folder_url_title + "</a>";
```