# 3- Building Charts

<p align="center">
  <img src="../images/step3.png" alt="Description of image" width="80%">
</p>

We start with creating the first ban chart for Total Sales. The procedure will be similar for Total Profit & Total Quantity bans.
<p align="center">
  <img src="../images/bans.png" alt="Description of image" width="40%">
</p>

## 3.1 Creating calculated fields & test

```
  IF YEAR([Order Date]) = 2023 THEN [Sales]
  END
```
```
  IF YEAR([Order Date]) = 2022 THEN [Sales]
  END
```

```
  YEAR([Order Date])
```



```
  IF YEAR([Order Date]) = [Select Year] THEN [Sales]
  END
```
```
  IF YEAR([Order Date]) = [Select Year]-1 THEN [Sales]
  END
```

```
  (SUM([CY Sales]) - SUM([PY Sales])) / SUM([PY Sales])
```

▲ 0.00%; ▼ -0.00%;

```
  IF SUM([CY Sales]) = WINDOW_MAX(SUM([CY Sales]))
  THEN SUM([CY Sales])
  ELSIF SUM([CY Sales]) = WINDOW_MIN(SUM([CY Sales]))
  THEN SUM([CY Sales])
  END
```



## 3.2 Building the charts

🛑*** Problem *** Continuous measures in visualization leads to a range of values in the title.

## 3.3 Formatting

### 3.3.1 Removing Lines & Grids
### 3.3.2 Cleaning up Axis & Headers
### 3.3.3 Coloring
### 3.3.4 Tooltip

💡*** Minimalism is key *** Excessive info on dashboard can distract users from the important data.

💡*** Tip *** Add your design colors to the 'Custom Colors' to reuse them in all charts.

CY Sales #212121
PY Sales #cecece
Max #1da2d0
Min #ff5500

```
  Sales of <MONTH(Order Date)>,<ATTR(Current Year)>:<SUM(CY Sales)>
  Sales of <MONTH(Order Date)>,<ATTR(Previous Year)>:SUM(PY Sales)
  Sales Differences:<AGG(% Diff Sales)>
  Highest/Lowest Sales:<AGG(Min/Max Sales)>
```

🗒️ *** Note *** Use tabs between values for alignment in tooltips.