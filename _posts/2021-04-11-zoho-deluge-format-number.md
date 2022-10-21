---
title: How To Format Number In Zoho Deluge
description: Zoho Deluge function to format numbers and return it as a string.
image: assets/images/deluge.svg
categories:
- Zoho
- ZohoCreator
- Zoho Deluge
layout: post
keywords:
- Text format
- Regex
- Low code
- Zoho Deluge
- Zoho

---
Let's learn how to format numbers as a string so that we can diplay the value in an HTML Page of Zoho Creator.

Working in Zoho Creator, we can often see that the displayed value of a field in a Page is not always what we expect. In such cases we need to use the power of Zoho Deluge Script to modify the rendering of such value.

We will show you below a small function to format a decimal value according to European Currency convention. Using Javascript, we would use [Intl.NumberFormat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat "Javascript NumberFormat object") object, but Zoho Deluge do not provide such capability ... yet.

### What We Need To Do

* Define the format we want to return.
  _In our specific case, we want to return a number format like `###.###,##`._
* Construct a suitable Regex string to use with the Deluge function `replaceAll()`.
  _Tip: we usually use_ [_RegExr_](https://regexr.com/ "Regex Learn, Build & Test") _to check our Regex and make some test._
* Check if the initial value is null. If not, return the formatted string, otherwise return the null value.

### Our Zoho Deluge Script

```javascript
String formatNumber(float decimal) 
{ 
  if(decimal != null) { 
    
    formattedString = decimal.round(2).toString()
      .replaceAll(".",",")
      .replaceAll("(?<!.\\d)(?<=\\d)(?=(?:\\d\\d\\d)+\\b)","."); 
    
    return formattedString; 
  
  } else { 
    
    return decimal; 
    
  } 
}
```

### Additional Resources

[_RegExr_](https://regexr.com/ "Regex Learn, Build & Test")