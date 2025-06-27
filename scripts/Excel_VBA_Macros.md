![image](https://github.com/user-attachments/assets/0ab271ac-f019-4f5f-af6b-d33aefc5838c)
---
The below VBA codes include two macros created to automate sales data analysis:

- ListFilterValues — generates lists of unique Regions, Products, or Sales Reps to help users select valid filter values easily.
- GenerateMultiValueFilterSalesReport_Formatted — creates a detailed, formatted sales report filtered by multiple Regions, Products, and Sales Reps with flexible partial matching.
These macros streamline filtering and reporting, saving time and improving accuracy.


```excel
Sub SalesSummaryReport()
    Dim wsData As Worksheet
    Dim wsSummary As Worksheet
    Dim lastRow As Long
    Dim totalSales As Double
    Dim totalQty As Long
    Dim avgPrice As Double
    
    Set wsData = ThisWorkbook.Sheets("sales")
    
    ' Create or clear Summary sheet
    On Error Resume Next
    Set wsSummary = ThisWorkbook.Sheets("Sales Summary")
    If wsSummary Is Nothing Then
        Set wsSummary = ThisWorkbook.Sheets.Add
        wsSummary.Name = "Sales Summary"
    Else
        wsSummary.Cells.Clear
    End If
    On Error GoTo 0
    
    lastRow = wsData.Cells(wsData.Rows.Count, "A").End(xlUp).Row
    
    totalSales = Application.WorksheetFunction.Sum(wsData.Range("J2:J" & lastRow)) ' Total column J
    totalQty = Application.WorksheetFunction.Sum(wsData.Range("H2:H" & lastRow)) ' Quantity column H
    avgPrice = Application.WorksheetFunction.Average(wsData.Range("I2:I" & lastRow)) ' Unit Price column I
    
    wsSummary.Range("A1").Value = "Sales Summary Report"
    wsSummary.Range("A3").Value = "Total Sales"
    wsSummary.Range("B3").Value = totalSales
    wsSummary.Range("A4").Value = "Total Quantity Sold"
    wsSummary.Range("B4").Value = totalQty
    wsSummary.Range("A5").Value = "Average Unit Price"
    wsSummary.Range("B5").Value = avgPrice
    
    MsgBox "Sales Summary Report created!"
End Sub



Sub GenerateMultiValueFilterSalesReport_Formatted()
    Dim wsSales As Worksheet, wsCustomers As Worksheet, wsProducts As Worksheet, wsSalesReps As Worksheet
    Dim wsReport As Worksheet
    Dim lastSalesRow As Long, lastCustRow As Long, lastProdRow As Long, lastRepRow As Long
    Dim i As Long, reportRow As Long
    Dim custDict As Object, prodDict As Object, repDict As Object
    Dim saleCustID As Variant, saleProdID As Variant, saleRepID As Variant
    Dim custRegion As String, prodName As String, repName As String
    Dim totalSales As Double, totalQty As Long
    Dim filterRegion As String, filterProduct As String, filterSalesRep As String
    Dim arrRegion() As String, arrProduct() As String, arrSalesRep() As String
    
    ' Set worksheets
    Set wsSales = ThisWorkbook.Sheets("sales")
    Set wsCustomers = ThisWorkbook.Sheets("customers")
    Set wsProducts = ThisWorkbook.Sheets("products")
    Set wsSalesReps = ThisWorkbook.Sheets("sales_reps")
    
    ' Get last rows
    lastSalesRow = wsSales.Cells(wsSales.Rows.Count, "A").End(xlUp).Row
    lastCustRow = wsCustomers.Cells(wsCustomers.Rows.Count, "A").End(xlUp).Row
    lastProdRow = wsProducts.Cells(wsProducts.Rows.Count, "A").End(xlUp).Row
    lastRepRow = wsSalesReps.Cells(wsSalesReps.Rows.Count, "A").End(xlUp).Row
    
    ' Create dictionaries for quick lookup
    Set custDict = CreateObject("Scripting.Dictionary")
    For i = 2 To lastCustRow
        custDict(wsCustomers.Cells(i, "A").Value) = wsCustomers.Cells(i, "C").Value ' CustomerID -> Region (col C)
    Next i
    
    Set prodDict = CreateObject("Scripting.Dictionary")
    For i = 2 To lastProdRow
        prodDict(wsProducts.Cells(i, "A").Value) = wsProducts.Cells(i, "B").Value ' ProductID -> ProductName (col B)
    Next i
    
    Set repDict = CreateObject("Scripting.Dictionary")
    For i = 2 To lastRepRow
        repDict(wsSalesReps.Cells(i, "A").Value) = wsSalesReps.Cells(i, "B").Value ' SalesRepID -> SalesRepName (col B)
    Next i
    
    ' Get filter inputs (can leave blank to ignore)
    filterRegion = LCase(Trim(InputBox("Enter Region(s) to filter by, separated by commas (leave blank to ignore):", "Filter Region")))
    filterProduct = LCase(Trim(InputBox("Enter Product Name(s) to filter by, separated by commas (leave blank to ignore):", "Filter Product")))
    filterSalesRep = LCase(Trim(InputBox("Enter Sales Rep Name(s) to filter by, separated by commas (leave blank to ignore):", "Filter Sales Rep")))
    
    ' Split inputs into arrays
    If filterRegion <> "" Then
        arrRegion = Split(filterRegion, ",")
        Call TrimArray(arrRegion)
    Else
        ReDim arrRegion(0)
        arrRegion(0) = ""
    End If
    
    If filterProduct <> "" Then
        arrProduct = Split(filterProduct, ",")
        Call TrimArray(arrProduct)
    Else
        ReDim arrProduct(0)
        arrProduct(0) = ""
    End If
    
    If filterSalesRep <> "" Then
        arrSalesRep = Split(filterSalesRep, ",")
        Call TrimArray(arrSalesRep)
    Else
        ReDim arrSalesRep(0)
        arrSalesRep(0) = ""
    End If
    
    ' Create or clear report sheet
    On Error Resume Next
    Set wsReport = ThisWorkbook.Sheets("Filtered Sales Report")
    If wsReport Is Nothing Then
        Set wsReport = ThisWorkbook.Sheets.Add
        wsReport.Name = "Filtered Sales Report"
    Else
        wsReport.Cells.Clear
    End If
    On Error GoTo 0
    
    ' Write headers
    wsReport.Range("A1").Value = "Sale ID"
    wsReport.Range("B1").Value = "Sale Date"
    wsReport.Range("C1").Value = "Customer ID"
    wsReport.Range("D1").Value = "Customer Region"
    wsReport.Range("E1").Value = "Product ID"
    wsReport.Range("F1").Value = "Product Name"
    wsReport.Range("G1").Value = "Sales Rep ID"
    wsReport.Range("H1").Value = "Sales Rep Name"
    wsReport.Range("I1").Value = "Quantity"
    wsReport.Range("J1").Value = "Unit Price"
    wsReport.Range("K1").Value = "Total"
    
    reportRow = 2
    totalSales = 0
    totalQty = 0
    
    ' Loop through sales and apply filters
    For i = 2 To lastSalesRow
        saleCustID = wsSales.Cells(i, "C").Value ' Customer ID col C
        saleProdID = wsSales.Cells(i, "D").Value ' Product ID col D
        saleRepID = wsSales.Cells(i, "E").Value ' Sales Rep ID col E
        
        custRegion = ""
        prodName = ""
        repName = ""
        
        If custDict.exists(saleCustID) Then custRegion = custDict(saleCustID)
        If prodDict.exists(saleProdID) Then prodName = prodDict(saleProdID)
        If repDict.exists(saleRepID) Then repName = repDict(saleRepID)
        
        ' Convert to lowercase for case-insensitive comparison
        custRegion = LCase(Trim(custRegion))
        prodName = LCase(Trim(prodName))
        repName = LCase(Trim(repName))
        
        ' Check filters with partial match
        If Not PartialMatchInArray(custRegion, arrRegion) Then GoTo SkipRow
        If Not PartialMatchInArray(prodName, arrProduct) Then GoTo SkipRow
        If Not PartialMatchInArray(repName, arrSalesRep) Then GoTo SkipRow
        
        ' Copy row data to report
        wsReport.Cells(reportRow, "A").Value = wsSales.Cells(i, "A").Value ' Sale ID
        wsReport.Cells(reportRow, "B").Value = wsSales.Cells(i, "B").Value ' Sale Date
        wsReport.Cells(reportRow, "C").Value = saleCustID
        wsReport.Cells(reportRow, "D").Value = custRegion
        wsReport.Cells(reportRow, "E").Value = saleProdID
        wsReport.Cells(reportRow, "F").Value = prodName
        wsReport.Cells(reportRow, "G").Value = saleRepID
        wsReport.Cells(reportRow, "H").Value = repName
        wsReport.Cells(reportRow, "I").Value = wsSales.Cells(i, "G").Value ' Quantity (col G)
        wsReport.Cells(reportRow, "J").Value = wsSales.Cells(i, "H").Value ' Unit Price (col H)
        wsReport.Cells(reportRow, "K").Value = wsSales.Cells(i, "I").Value ' Total (col I)
        
        ' Update totals
        totalSales = totalSales + wsSales.Cells(i, "I").Value
        totalQty = totalQty + wsSales.Cells(i, "G").Value
        
        reportRow = reportRow + 1
SkipRow:
    Next i
    
    ' Write summary below data
    Dim summaryRow As Long
    summaryRow = reportRow + 2
    wsReport.Cells(summaryRow, "A").Value = "Summary"
    wsReport.Cells(summaryRow + 1, "A").Value = "Total Sales"
    wsReport.Cells(summaryRow + 1, "B").Value = totalSales
    wsReport.Cells(summaryRow + 2, "A").Value = "Total Quantity Sold"
    wsReport.Cells(summaryRow + 2, "B").Value = totalQty
    wsReport.Cells(summaryRow + 3, "A").Value = "Average Unit Price"
    If totalQty > 0 Then
        wsReport.Cells(summaryRow + 3, "B").Value = totalSales / totalQty
    Else
        wsReport.Cells(summaryRow + 3, "B").Value = 0
    End If
    
    ' --- Formatting ---
    
    ' Format header row
    With wsReport.Range("A1:K1")
        .Font.Bold = True
        .Font.Size = 12
        .Interior.Color = RGB(221, 235, 247) ' Light blue
        .HorizontalAlignment = xlCenter
        .VerticalAlignment = xlCenter
    End With
    
    ' Auto-fit columns
    wsReport.Columns("A:K").AutoFit
    
    ' Format columns
    wsReport.Columns("B").NumberFormat = "mm/dd/yyyy" ' Sale Date
    wsReport.Columns("I").NumberFormat = "0" ' Quantity as integer
    wsReport.Columns("J:K").NumberFormat = "$#,##0.00" ' Unit Price and Total as currency
    
    ' Add borders around data range
    Dim lastDataRow As Long
    lastDataRow = wsReport.Cells(wsReport.Rows.Count, "A").End(xlUp).Row
    With wsReport.Range("A1:K" & lastDataRow).Borders
        .LineStyle = xlContinuous
        .Weight = xlThin
        .ColorIndex = xlAutomatic
    End With
    
    ' Format summary section
    With wsReport.Range("A" & summaryRow & ":B" & (summaryRow + 3))
        .Font.Bold = True
        .Columns(2).NumberFormat = "$#,##0.00"
        .Interior.Color = RGB(242, 242, 242) ' Light gray background
    End With
    
    ' Freeze top row
    wsReport.Activate
    ActiveWindow.SplitRow = 1
    ActiveWindow.FreezePanes = True
    
    ' Add autofilter to header row
    wsReport.Range("A1:K1").AutoFilter
    
    ' Conditional formatting: highlight Total > 1000 in light green
    Dim cfRange As Range
    Set cfRange = wsReport.Range("K2:K" & lastDataRow)
    cfRange.FormatConditions.Delete
    cfRange.FormatConditions.Add Type:=xlCellValue, Operator:=xlGreater, Formula1:="1000"
    cfRange.FormatConditions(1).Interior.Color = RGB(198, 239, 206) ' Light green
    
    MsgBox "Filtered Sales Report created with your criteria and formatted nicely."
End Sub

' Helper function to trim spaces from array elements
Sub TrimArray(arr() As String)
    Dim i As Long
    For i = LBound(arr) To UBound(arr)
        arr(i) = Trim(arr(i))
    Next i
End Sub

' Helper function for partial match: returns True if any filter value is contained in the data value
Function PartialMatchInArray(dataVal As String, arr() As String) As Boolean
    Dim i As Long
    Dim filterVal As String
    
    ' If filter array is empty or contains only empty string, accept all
    If UBound(arr) = 0 And arr(0) = "" Then
        PartialMatchInArray = True
        Exit Function
    End If
    
    For i = LBound(arr) To UBound(arr)
        filterVal = arr(i)
        If filterVal <> "" Then
            If InStr(1, dataVal, filterVal, vbTextCompare) > 0 Then
                PartialMatchInArray = True
                Exit Function
            End If
        End If
    Next i
    
    PartialMatchInArray = False
End Function

```

---

---
