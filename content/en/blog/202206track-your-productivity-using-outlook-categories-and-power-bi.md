---
title: "Track Your Productivity Using Outlook Categories and Power BI (Desktop)"
date: 2022-06-12T14:49:05-05:00
draft: false
author: "Lindsay Shelton"
tags: ["how to","microsoft outlook","power bi","outlook categories","productivity"]
thumbnail: /images/track-your-productivity-using-outlook-categories-and-power-bi1.png
hidePageThumbnail: true
toc: true

---

## Basic Category Visualization
1. Start creating data to gather by creating your Outlook categories and categorizing all of your appointments and meetings over a period of time.
2. Download Power BI Desktop for free and open it up
3. Click “Get Data”
4. Search for “Exchange” and connect to “Microsoft Exchange”
5. Put in your email address that you want to connect
6. The first time you do it, you will have a screen to connect via Exchange or Microsoft. I recommend picking Microsoft and clicking “Connect”
7. You now select the “Calendar” checkbox
8. Click “Transform Data”
9. Click “Choose columns -> Choose columns”
10. Check the “Select all” box to deselect all of the columns
11. Select the four following columns: Subject, Start, End, Category
12. On the Category column, click the button with the two outward arrows
13. Click “Extract values”
14. Click “Ok” without selecting a delimiter
15. Go to the Add Column menu
16. Click “Add Custom Column”
17. Call the custom column “Duration”
18. Put the following code in: = [End] – [Start]
19. Click “Ok”
20. Right click the header of your new Duration column
21. Click “Transform” and “Total Minutes”
22. Click on the down arrow on the Start column
23. Hover over Date/Time Filters and then go to what you want to select. In my example, Date/Time Filters -> Week -> This Week
24. Click “Close and Apply”
25. Add charts as desired
26. When you’ve created a dashboard you like, save the file
27. You can click “Add a text box” and put a title on your dashboard
28. To share with others easily, click “File -> Export -> Export as PDF”
29. To change your dates, click “Transform Data”
30. Either clear your Date/Time Filter or click the red X on the right side next to the step where you filtered the rows
31. To deal with holidays, you can remove specific rows or just right click and exclude data without a category from each visualization

![Placeholder](/images/track-your-productivity-using-outlook-categories-and-power-bi1.png)

## Adding Value Add vs Non Value Add Comparison
1. Click “New Measure” in Power BI
2. Type in this formula: Value Add Activities = sum([Column: Duration])
3. Click “New Measure” again
4. Type in this formula: Non Value Add Activities = 2400 – ([Value Add Activities])
5. Create a new visualization and only select the two new measures, Value Add Activities and Non Value Add Activities

![Placeholder](/images/track-your-productivity-using-outlook-categories-and-power-bi2.png)

## NEW: Tracking Longitudinal Growth Over Time
Our client had an additional ask – how do we see in one visual how someone is performing in the categories over a span of time? Here are the steps I walked through to create my end visual:

1. (Optional) Add a new page at the bottom by clicking the yellow/orange plus sign
2. Click “Data” on the left
3. Click “New Table”
4. Clear the field and paste in this code for DimDate (dates can be changed as needed):
DimDate = ADDCOLUMNS(CALENDAR(“01/01/2022″,”31/12/2022″),”Month – Year”, FORMAT([Date],”mm – yy”),”SortMonth”,FORMAT([Date],”yyyymm”))
5. Hit the green checkmark to create this new table
6. Click “Report” on the left
7. Click “Transform Data”
8. Click the Start column
9. Select “Add Column” menu
10. Click Extract -> Text Before Delimiter
11. Type in a single space as the delimiter
12. Click “Ok”
13. Select the new Text Before Delimiter column
14. Click Date -> Parse
15. Remove the date filter on the Start column by clicking the red X next to “Filtered Rows” under “Applied Steps” and then “Delete”
16. Add a date filter on the Start column of Year -> This Year
17. To remove holidays, go back to the “Home” menu
18. Click “Keep Rows -> Keep Range of Rows”
19. Put in the range of rows with valid meetings

NOTE that during my session, Daryl Rasmussen suggested creating a “Holiday” category and then filtered to exclude that category, so another method to try out!

20. Click “Close and Apply”
21. Click “New Measure”
22. Clear the field and paste in this code for Value Add Activities Last 3 Months:
Value Add last 3 months = CALCULATE(SUM(‘Calendar'[Duration]),FILTER(ALL(‘Calendar'[Parse]),’Calendar'[Parse]<=MAX(DimDate[Date])&&’Calendar'[Parse] >= EDATE(MAX(DimDate[Date]), -3)))

NOTE – this is for the last three months. To make it more or less, change two things in the code – the name of it, and that “-3” at the very end to the count of months you want to calculate for

23. Create a Slicer visualization and drag to the left of the screen
24. With that visualization selected, check the boxes under Fields for DimDate (expand it) -> Month-Year
25. (Optional) Increase the font size by clicking Format under Visualizations (the roller paint brush), Items, and Text Size – adjust accordingly
26. Created a Stacked Column Chart visualization
27. With that visualization selected, under the Calendar table, check the boxes for “Parse”, “Value Add Last 3 Months”, and “Categories”
28. Under the Visualizations section, where it says Axis -> Parse, click the X next to “Year”, “Quarter”, and “Day”
29. Select your current month in the slicer and start playing from there!

![Placeholder](/images/track-your-productivity-using-outlook-categories-and-power-bi3.png)

If you have any further questions, please feel free to reach out to me at @lshelton_tech on Twitter and I will do my best to help!

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-CN3PDT3T20"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-CN3PDT3T20');
</script>