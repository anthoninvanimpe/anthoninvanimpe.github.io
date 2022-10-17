---
title: 'Zoho Deluge'
description: 'Function to get the correct week number of the year.'
tags: [Zoho, Zoho Deluge, ZohoCreator]
layout: article
---

```javascript
// Function found on Help forum from Zoho

int Utilities.getWeekOfYear_ISO(date Date_In)
{
	//The Zoho function getWeekOfYear does not follow the same method as the ISO-8601 standard
	//the details of the standard can be found at https://en.wikipedia.org/wiki/ISO_week_date#Calculating_the_week_number_of_a_given_date
	//This function will return the ISO week number
	//Herb Wexler Jan 8 2016
	What_Year = Date_In.getYear();
	Last_Year = What_Year - 1;
	Ordinal_Date = Date_In.getDayOfYear();
	//zoho uses Sunday = 1 instead of the standard Monday = 1
	Day_Of_Week = Date_In.getDayOfWeek() - 1;
	if(Day_Of_Week = 0)
	{
		Day_Of_Week = 7;
	}
	Week_Number_ISO_temp = (Ordinal_Date - Day_Of_Week + 10) / 7;
	Week_Number_ISO = Week_Number_ISO_temp.toLong();
	if(Week_Number_ISO = 0)
	{
		End_Prev_Year = ("12/31/" + Last_Year).toDate();
		Ordinal_Date = End_Prev_Year.getDayOfYear();
		//zoho uses Sunday = 1 instead of the standard Monday = 1
		Day_Of_Week = End_Prev_Year.getDayOfWeek() - 1;
		if(Day_Of_Week = 0)
		{
			Day_Of_Week = 7;
		}
		Week_Number_ISO_temp = (Ordinal_Date - Day_Of_Week + 10) / 7;
		Week_Number_ISO = Week_Number_ISO_temp.toLong();
		//info Ordinal_Date + " " + End_Prev_Year + " " + Day_Of_Week +" " + Week_Number_ISO_temp;
	}
	return Week_Number_ISO;
}
```

## Get Week Numbers between two dates

```javascript
list Timeline.weeksList(date start, date end, list weeksList)
{
	if(thisapp.Utilities.getWeekOfYear_ISO(start) == thisapp.Utilities.getWeekOfYear_ISO(end))
	{
		if(weeksList.contains(thisapp.Utilities.getWeekOfYear_ISO(start)))
		{
			return weeksList;
		}
		else
		{
			weeksList.add({thisapp.Utilities.getWeekOfYear_ISO(start),start.toString("MMM")});
			return weeksList;
		}
	}
	else if(thisapp.Utilities.getWeekOfYear_ISO(start) > thisapp.Utilities.getWeekOfYear_ISO(end))
	{
		return thisapp.Timeline.weeksList(end,end,weeksList);
	}
	else
	{
		month = start.toString("MMM");
		listWeekAndMonth = {thisapp.Utilities.getWeekOfYear_ISO(start),month};
		weeksList.add(listWeekAndMonth);
		newDate = start.addWeek(1);
		return thisapp.Timeline.weeksList(newDate,end,weeksList);
	}
}
```