---
title: 'Automate Workdrive Folder Creation'
image: 'assets/images/workdrive-icon.png'
description: 'Zoho Deluge function to create a folder inside Zoho Workdrive.'
categories: [Zoho, Zoho Workdrive, Zoho Deluge]
layout: post
---

test

```javascript
year = input.DATE1.getYear();
supplier_name = zoho.crm.getRecordById("Vendors",input.SUPPLIER_2.toLong()).get("Vendor_Name");
test_folder_name = input.PRODUCT_CATEGORY + " -Attn. " + input.ATTN_TO + " - " + supplier_name + "(" + input.TEST_STANDARD + ")";
headers=Map();
headers.put("Accept","application/vnd.api+json");

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