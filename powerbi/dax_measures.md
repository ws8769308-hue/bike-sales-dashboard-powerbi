# DAX

```
Gross_Sales  = SUMX(SalesT, SalesT[Quantity] * RELATED('Product'[Price]))
Disc._Value  = SUMX(SalesT, SalesT[Quantity] * SalesT[Discount] * RELATED('Product'[Price]))
Net_Sales    = [Gross_Sales] - [Disc._Value]
```

Calculated columns on `SalesT`:

```
Gross Sales = RELATED('Product'[Price]) * SalesT[Quantity]
Disc_Value  = SalesT[Discount] * SalesT[Gross Sales]
Net Sales   = SalesT[Gross Sales] - SalesT[Disc_Value]
```
