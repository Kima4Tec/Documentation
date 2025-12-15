#Power Bi

### Installér Power Bi på engelsk:
https://www.microsoft.com/en-us/download/details.aspx?id=58494

## De tre vigtigste Power BI-elementer

### Power BI består grundlæggende af:

**✔ Desktop**

- Til at udvikle rapporter og datamodeller.

**✔ Power BI Service (web)**

- Til at dele dashboards og rapporter, samt automatiske opdateringer.

**✔ Gateway**

- Hvis dine data ligger lokalt (SQL Server, Excel på netværksdrev osv.), skal du bruge Gateway til automatisk refresh.


## DAX
### Eksempler på målinger fra FirstReport:
```
Total Sales YTD = 
CALCULATE([Total Sales Amount],DATESYTD('Calendar'[Date]))
```

```
Total Sales Amount = SUMX('Internet Sales', 'Internet Sales'[Freight]+'Internet Sales'[Tax Amt]+'Internet Sales'[Sales Amount]) 
```
```
Total Quantity = 
SUM ('Internet Sales'[Order Quantity])
```

```
Total Freight = 
SUM ( 'Internet Sales'[Freight])
```
