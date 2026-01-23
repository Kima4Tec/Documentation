# Power Bi

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

### M: get data from api with token
```M
= (Query as text) => let
        EndPoint = "statistik",
        Url = "https://api.navn.dk/api/v1/",
        Token = "Indsæt API-nøgle her",
        auth_key = "Bearer "&Token,
        header= [
            #"Authorization" = auth_key,
            #"Content-Type" = "application/json"
        ],
        body = Text.ToBinary(Query),
        rawdata = Web.Contents(Url&EndPoint, [Headers=header, Content=body]),
        jsonData = Json.Document(rawdata),
        toTable = Table.FromList(jsonData, Splitter.SplitByNothing(), null, null, ExtraValues.Error)
    in
        toTable
```
