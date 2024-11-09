# 3- Building Charts

## ** 3.1 KPI Overview and Sales Trends **

<p align="center">
  <img src="../images/step3.png" alt="Description of image" width="80%">
</p>

We start with creating the first ban chart for Total Sales. The procedure will be similar for Total Profit & Total Quantity bans.
<p align="center">
  <img src="../images/bans.png" alt="Description of image" width="40%">
</p>

By dropping Sales in the Text Marks and selecting Order Date as our rows, we see that we have the total sales for years 2020 to 2023.
<p align="center">
  <img src="../images/3.1.png" alt="Description of image" width="40%">
</p>

### 3.1.1 Creating calculated fields & test

We will create a calculated field regarding the KPI requirements so that we can only have the sales of the current year. We'll call this 'CY Sales' and we define it as follows:

```
  IF YEAR([Order Date]) = 2023 THEN [Sales]
  END
```
<p align="center">
  <img src="../images/3.2.png" alt="Description of image" width="40%">
</p>

We can create a similar calculated field for the sales of the previous year. We'll call this 'PY Sales' and define it accordingly:

```
  IF YEAR([Order Date]) = 2022 THEN [Sales]
  END
```

<p align="center">
  <img src="../images/3.3.png" alt="Description of image" width="40%">
</p>

Now, how can we make the dashboard dynamic? By using parameters to allow users to check historical data to select any desired year.
We start off with creating a calculated field that returns the value of the year from Order Date called 'Order Date(Year)', defined as follows:

```
  YEAR([Order Date])
```
Now, we can create a parameter called 'Select Year':

<p align="center">
  <img src="../images/3.4.png" alt="Description of image" width="40%">
</p>

Then, it's time to link our parameter to our calculated fields by editing them as follows:


```
  IF YEAR([Order Date]) = [Select Year] THEN [Sales]
  END
```
```
  IF YEAR([Order Date]) = [Select Year]-1 THEN [Sales]
  END
```
We've calculated the amount of sales for the current and previous year. Now, it's the time to calculate their differences in percentage. We call it '% Diff Sales', defined as follows:

```
  (SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales])
```
We'll put all the bans info in the title part and include the sparkline chart as well in one sheet.
```
  Total Sales
  <SUM(CY Sales)>
  <AGG(% Diff Sales)> vs. PY
```

We can provide the following custom format for difference percentage KPI:

▲ 0.0%; ▼ -0.0%;

<p align="center">
  <img src="../images/3.5.png" alt="Description of image" width="40%">
</p>






### 3.1.2 Building the charts

🛑*** Problem *** Continuous measures in visualization leads to a range of values in the title.

<p align="center">
  <img src="../images/3.6.png" alt="Description of image" width="40%">
</p>

Note that how after adding the sparkline chart on top of each other, our bans info broke. We'll put a pin on this and will take care of it later. First, let's add the min/max circles in our sales charts. We create a calculated field called 'Min/Max Sales', defined as follows:


```
  IF SUM([CY Sales]) = WINDOW_MAX(SUM([CY Sales]))
  THEN SUM([CY Sales])
  ELSIF SUM([CY Sales]) = WINDOW_MIN(SUM([CY Sales]))
  THEN SUM([CY Sales])
  END
```
Remember to choose dual axis so that the circles would be placed on top of the line charts rather than separated.

<p align="center">
  <img src="../images/3.7.png" alt="Description of image" width="40%">
</p>

Now,to fix the problem we faced in our bans section when we added the sparkline charts, we can do the following LOD trick(Level of Detail):

We edit our marks by wrapping them in curly brackets,{}, and then, edit our bans info by chosing the new marks:

```
  Total Sales
  <SUM({SUM([CY Sales])})>
  <SUM({[% Diff Sales]})>vs. PY

```

Everything is back to normal now!

<p align="center">
  <img src="../images/3.8.png" alt="Description of image" width="40%">
</p>


### 3.1.3 Formatting

#### 3.1.3.1 Removing Lines & Grids
#### 3.1.3.2 Cleaning up Axis & Headers

💡*** Minimalism is key *** Excessive info on dashboard can distract users from the important data.

#### 3.1.3.3 Coloring
#### 3.1.3.4 Tooltip



💡*** Tip *** Add your design colors to the 'Custom Colors' to reuse them in all charts.

Colors:

CY Sales #212121
PY Sales #cecece
Max #1da2d0
Min #ff5500

* We'll edit the tooltip for nicer formatting as follows:

```
  Sales of <MONTH(Order Date)>,<ATTR(Current Year)>:<SUM(CY Sales)>
  Sales of <MONTH(Order Date)>,<ATTR(Previous Year)>:SUM(PY Sales)
  Sales Differences:<AGG(% Diff Sales)>
  Highest/Lowest Sales:<AGG(Min/Max Sales)>
```

🗒️ *** Note *** Use tabs between values for alignment in tooltips.

<p align="center">
  <img src="../images/3.9.png" alt="Description of image" width="40%">
</p>

## ** 3.2 Product Subcategory Comparison **

<p align="center">
  <img src="../images/3.10.png" alt="Description of image" width="40%">
</p>

### 3.2.1 Creating Calculated Fields & Tests 

*** Note *** Typically, only first chart requires a lot of creation of calculated fields.

In our case, we can jump into building the subcategory comparison charts.

### 3.2.2 Building Charts

We can easily create the bar-in-bar charts by dragging 'SUM(PY Sales)' and 'SUM(CY Sales)' to our rows and 'Sub-Category' to our columns. We then need to use dual axis and synchronize their axis. We can also add our 'SUM(CY Profit)' to have our current year profit chart next to our sales bar-in-bar chart.

### 3.2.3 Formatting

We can add a custom icon next to each sub-category where our current year sales is less than the previous year sales. Well define a calculated field called 'KPI CY Less PY' as follows:

```
  IF SUM([CY Sales]) < SUM([PY Sales]) THEN '⬤'
  ELSE ''
  END
```

We now only need to drop AGG(KPI CY Less PY) to our rows to see which categories are underperforming. We can sort the comparisons descending based on the 'CY Sales'.

<p align="center">
  <img src="../images/3.11.png" alt="Description of image" width="40%">
</p>


### 3.2.4 Tooltip

We can edit the tooltip using the following attributes:

```
  Sub-Category: <Sub-Category>
  Sales of <ATTR(Current Year)>:  <SUM(CY Sales)>
  Sales of <ATTR(Previous Year)>: <SUM(PY Sales)>
  % Diff Sales: <AGG(% Diff Sales)>
  Profit of <ATTR(Current Year)>: <SUM(CY Profit)>

```

<p align="center">
  <img src="../images/3.12.png" alt="Description of image" width="40%">
</p>


## 3.3 Weekly Trends for Sales and Profit

<p align="center">
  <img src="../images/3.13.png" alt="Description of image" width="40%">
</p>

### 3.3.1 Building Charts
### 3.3.2 Formatting

```
  IF SUM([CY Sales]) > WINDOW_AVG(SUM([CY Sales]))
  THEN 'above'
  ELSE 'below'
  END
```

```
  above: #1da2d0
  below: #ff5500
```

### 3.3.3 Tooltip

```
  Nr. of Week:  <WEEK(Order Date)>
  Sales of <ATTR(Current Year)>:  <SUM(CY Sales)>
  <AGG(KPI Sales Avg)> the average
  Profit of <ATTR(Current Year)>: <SUM(CY Profit)>
  <AGG(KPI Profit Avg)> the average

```

<p align="center">
  <img src="../images/3.17.png" alt="Description of image" width="40%">
</p>

<p align="center">
  <img src="../images/3.18.png" alt="Description of image" width="40%">
</p>


<p align="center">
  <img src="../images/3.19.png" alt="Description of image" width="40%">
</p>

