---
tag: dax, bestPractice, performance, powerBI
---
[[ReadItLater]] [[Article]]

# [DAX Best Practices | MAQ Software Insights](https://maqsoftware.com/insights/dax-best-practices)

## DAX Best Practice Guide

Last Updated: August 27, 2021

This guide enables you to speed up your Power BI reports by optimizing their back-end code. As the 2021 Microsoft Power BI Partner of the Year, we are recognized for our expertise in implementing business intelligence and analytics solutions.

We have compiled these best practices based on our experience, including:

1.  [Improving DAX Syntax](https://maqsoftware.com/#syntax)
2.  [Optimizing DAX Functions](https://maqsoftware.com/#functions)
3.  [Common Mistakes To Avoid](https://maqsoftware.com/#mistakes)

## Before you start:

Tip #1: Always clear your DAX cache before optimizing DAX  
Your DAX cache builds up from internal VertiPaq queries. You can clear your cache from within DAX Studio. Resetting your cache enables you to effectively measure performance gains.

## Improving DAX Syntax

1.  **Use DAX Formatter to format your code**

Formatted code is easier to read and maintain. DAX Formatteris a free tool that enables you to transform raw DAX into readable code.

3.  **Use the DISTINCT() and VALUES() functions consistently**

Power BI adds a blank value to a column if it finds a referential integrity violation. When making direct queries, Power BI adds a blank value to columns because it cannot check for violations.

The DISTINCT() and VALUES() functions are different:

1.  DISTINCT(): Does not return any blanks added due to an integrity violation. The DISTINCT() function includes a blank only if it is part of the original data.
2.  VALUES(): Returns both blanks from the original data and blanks that Power BI added due to referential integrity violations.

Be consistent in your use of the DISTINCT() and VALUES() functions throughout the entire report. Otherwise, you’ll have inconsistent values for blank columns. We recommend using the VALUES() function if blank values are not an issue.

8.  **Add column and measure references in your DAX expressions**

To ensure your DAX can be understood and used by anyone, you need to eliminate ambiguity. By adding column and measure references, you ensure that anyone can easily read your DAX at a glance. We recommend always using fully qualified column references and never using fully qualified measure references. That way, you can quickly differentiate between a column or a measure based on whether it’s fully qualified.

Adding column and measure references also ensures that expressions work when a measure home table is changed.

With col reference: `Profit = Orders[Sales] - Orders[Cost]`

Without col reference: `Profit = [Sales] - [Cost]`

## Optimizing DAX Functions

1.  **Use ISBLANK() instead of =Blank() check**

Use the built-in function ISBLANK() to check for any blank values instead of using the comparison operator = Blank(). While = Blank() returns ‘True’ value for either blank values or empty strings, IsBlank exclusively checks for blanks.

3.  **Use = 0 instead of checking for ISBLANK() || = 0**

The BLANK value in Power BI is associated with the base value of a column’s data type. The BLANK value corresponds to “0” for integers, “(empty string)” for string columns, and “1–1–1900” for date fields.

ISBLANK() || = 0 enacts two checks: first it checks if a column is BLANK, then it checks for zeroes. = 0 performs both checks at once, improving calculation speed.

To check exclusively for zero, use the IN operator.

7.  **Use SELECTEDVALUE() instead of HASONEVALUE()**

It is common practice to use HASONEVALUE() to check if there is only one value present in a column after applying slicers and filters. However, when you do this, you also have to use the VALUES(ColumnName) DAX function to retrieve that single value.

SELECTEDVALUE() performs the above steps internally. It automatically retrieves the single value if there is one and returns a blank if there are multiple values available.

10.  **Use SELECTEDVALUE() instead of VALUES()**

The VALUES() function returns an error if it encounters multiple values. Often, users address the error using Error functions, which negatively affects performance.

Instead of using VALUES(), use SELECTEDVALUE(). The SELECTEDVALUE() function returns a blank if it encounters multiple values (instead of an error).

13.  **Use variables instead of repeating measures inside the IF branch**

Incorrect DAX: `Ratio = IF([Total Rows] > 10, SUM(Revenue) /[Total Rows], 0)`

Here, measures are calculated continuously, meaning the \[Total Rows\] expression is calculated twice: first for the condition check, then for the true condition expression.

Correct DAX: `VAR totalRows = [Total Rows]; Ratio = IF(totalRows > 10, SUM(Revenue) / totalRows,0)`

Instead of calculating the same expression multiple times, you can store the resulting measure value in a variable. You can use a variable reference wherever required. The same variable process applies to all instances where you call the same measure. Variables can help you avoid repetitive functions.

Note: Be aware that variables are actually constants.

19.  **Use DIVIDE() instead of /**

/ raises an exception if the denominator is zero. The DIVIDE() function internally performs a check to validate whether the denominator is zero. If it is, it returns the value specified in a third (extra) parameter.

For “invalid denominator” cases, you need to use the IF condition when using the “/” operator. The DIVIDE() function performs IF checks internally.

Note: If you are certain the denominator value is not zero, it is better to use the “/” operator without an IF check.

23.  **Use KEEPFILTERS() instead of FILTER(T)**

The FILTER function overrides any existing set of filters on a column applied via slicers. The KEEPFILTER function does not override the existing set of filters. Instead, it uses the intersection of values present in both, thus maintaining the current context. Use it when you want to maintain any filters applied by slicers or at a report level while performing calculations.

25.  **Use FILTER(all(ColumnName)) instead of FILTER(values()) or FILTER(T)**

To calculate measures independent of any filters applied to a column, combine the All(ColumnName) function with the FILTER function instead of using Table or VALUE(). For example: `CALCULATE([Total Sales], FILTER(ALL(Products[Color]), Color = ‘Red’))`

Use ALL along with the FILTER function if there is no need to keep the current context. Directly applying filters using expressions instead of the FILTER function behaves in the same way as mentioned above. This method internally translates using the ALL function in the filter. For example: `CALCULATE([Total Sales], FILTER(ALL(Products[Color]), Color = ‘Red’))`

It is always better to apply filters to the desired column than to the whole table, as this can easily scale.

References: [hpbidax](https://pbidax.wordpress.com/2016/05/22/simple-filter-in-dax-measures/) and [sqlbi](https://www.sqlbi.com/articles/filter-arguments-in-calculate/)

30.  **Use COUNTROWS instead of COUNT**

In Power BI, you can either use the COUNT function to count column values, or the COUNTROWS function to count table rows. Both functions achieve the same result if the counted column contains no BLANKs.

COUNTROWS is usually the better option for three reasons:

1.  It's more efficient, and will perform better
2.  It doesn't consider BLANKs
3.  The formula intention is clearer and self-descriptive

For example: `Sales Orders = COUNT(Sales [OrderDate])`

versus `Sales Orders = COUNTROWS(Sales)`

Reference: [DAX-CountRows](https://docs.microsoft.com/en-us/power-bi/guidance/dax-countrows)

37.  **Use SEARCH() with the last parameter**

The SEARCH() DAX function accepts the last parameter as the value that the query must return if the search string is not found. You should always use the SEARCH() function instead of using Error functions along with SEARCH().

39.  **ALL vs. ALLExcept**

ALLEXCEPT() behaves exactly like ALL(), VALUES() as long as the “exempted” columns are columns on the pivot. ALLEXCEPT() does NOT preserve pivot context on columns that are not on the pivot. Use ALL() instead of ALLEXCEPT() when using VALUES()

## Common Mistakes To Avoid

1.  **Do not change BLANK values to zeros or other strings**

It is common practice to replace blanks with zeros or other strings. However, Power BI automatically filters all rows with blank values. When viewing results from tables with large amounts of data, this limits the result set and improves performance.

If you replace blanks, Power BI does not filter the unwanted rows, negatively affecting performance.

4.  **Use (a-b)/b along with variables instead of a/b — 1 or a/b\*100–100**

It is common practice to use a/b — 1 to calculate a ratio and avoid duplicate measure calculations. However, you can achieve the same performance by using variables and using (a-b)/b to calculate the ratio.

If both a and b are blank values, then (a-b)/b returns a blank value and Power BI will filter the values out. a/b — 1 would return -1 as the result because both a and b are integers.

Reference: [sqlbi](https://www.sqlbi.com/articles/optimizing-if-conditions-using-variables/)

8.  **Stop using IFERROR() and ISERROR()**

The IFERROR() and ISERROR() functions were widely used in Excel when applying the FIND() and SEARCH() functions. They were necessary because FIND() and SEARCH() returned errors if the query did not obtain the required result.

The IFERROR() and ISERROR() functions force the Power BI engine to perform a step-by-step execution of each row to check for errors. There is currently no method to directly state which row returned the error.

The FIND() and SEARCH() DAX functions provide an extra parameter that the query can pass. The parameter is returned if the search string is not present. The FIND() and SEARCH() DAX functions check if more than one value is returned. They also ensure nothing is divided by zero.

You can avoid using the FIND() and SEARCH() DAX functions altogether by using situationally appropriate DAX functions such as DIVIDE() and SELECTEDVALUE(). The DIVIDE() and SELECTEDVALUE() functions perform error check internally and return the expected results.

Remember: You can *always* use DAX expressions in such a way that they never return an error.

14.  **Do not use scalar variables in SUMMARIZE()**

The SUMMARIZE() function is traditionally used to group columns and return resulting aggregations. However, the SUMMARIZECOLUMNS() function is newer and more optimized. Use that instead.

Only use SUMMARIZE() for grouped elements of a table that don’t have any associated measures or aggregations. For example: `SUMMARIZE(Table, Column1, Column2)`

17.  **Avoid using the AddColumns() function inside measure expressions**

Measures are calculated iteratively by default. If measure definitions use iterative functions such as AddColumns(), Power BI creates nested iterations, which negatively affect report performance.

19.  **Check if you can convert your column to a Boolean column**

If there are only two distinct values in a column, check if the column can be converted to use a Boolean data type (true/false). Boolean data types speed up processing when you have a large number of rows.

21.  **Avoid filtering on string columns**

Instead, use ID columns for filtering. For example, if you need to filter by sales location, assign each location a numeric ID. This means you filter by integer columns rather than string columns. Now, you can take advantage of the VertiPaq engine, which uses value encoding to reduce the memory of a column.

Note: Value encoding only works on integers.

24.  **Work upstream, if possible**

If certain calculations require complex DAX formulae, or if multiple filters are applied repeatedly in DAX measures and calculations, consider creating calculated columns or flags in the back end.

## References

-   [Data Analysis Expressions (DAX) Reference](https://docs.microsoft.com/en-us/dax/data-analysis-expressions-dax-reference) – Microsoft Corporation, updated regularly
-   [DAX function reference](https://docs.microsoft.com/en-us/dax/dax-function-reference) – Microsoft Corporation, published July 28, 2021
-   [Optimization guide for Power BI](https://docs.microsoft.com/en-us/power-bi/power-bi-reports-performance) – Microsoft Corporation, published April 2, 2021

![](BPG007-main.jpg..jpg)