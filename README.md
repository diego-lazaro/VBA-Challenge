# VBA-ChallengeSub vba_test_example()
'loop through all sheets
'------------------------------------------
For Each ws In Worksheets

'Delcaring all of our variables for the project

Dim WorksheetName As String

        WorksheetName = ws.Name
        
'create last row so code can loop through all worksheets

        Dim LastRow As Double

                LastRow = ws.Cells(Rows.Count, 1).End(xlUp).Row + 1
                
Dim Ticker As String

Dim SummaryTable As Double

        SummaryTable = 2
        
Dim PercentageChange As Double
       
Dim YearlyChange As Double
    
Dim StockOpen As Double

    StockOpen = ws.Cells(2, 3).Value
    
Dim StockClosed As Double

    StockClosed = ws.Cells(2, 6).Value
    
Dim TotalVolume As Double

    TotalVolume = 0
                
'start loop for ticker column
For i = 2 To LastRow

    If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
        
           Ticker = ws.Cells(i, 1).Value

'------------------------------
'create total stock volume

TotalVolume = TotalVolume + Cells(i, 7).Value

'create range i want to input the data into

ws.Range("I" & SummaryTable).Value = Ticker

ws.Range("L" & SummaryTable).Value = TotalVolume

'create value for StockClosed inside loop
'--------------------------------------------

    StockClosed = ws.Cells(i, 6).Value
    
'create value for YearlyChange
'--------------------------------------------
    
    YearlyChange = (StockClosed - StockOpen)
    
'needed to add decimal format to yearly change, kept getting to many decimal places
'-----------------------------------------------------
     ws.Range("J" & SummaryTable).NumberFormat = "0.00"
     
    ws.Range("J" & SummaryTable).Value = YearlyChange
    
End If

'color code for outcomes.

If ws.Range("J" & SummaryTable).Value <= 0 Then

       ws.Range("J" & SummaryTable).Interior.ColorIndex = 3

            Else

        ws.Range("J" & SummaryTable).Interior.ColorIndex = 4

     End If

'Kept getting error 13 and 1004. i had to add more to my branch. my code worked for my first branch on my loop. afterwards, kept getting erros. errrr. finally able to debug

    If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value And StockOpen <> 0 Then
    
            PercentageChange = YearlyChange / StockOpen
            
            
'format percentage and decimals places and have loop reset for next loop
'Format for Percentages and Decimal Places in Table and reseting values for next loop

        ws.Range("K" & SummaryTable).NumberFormat = "0.00%"
        
        ws.Range("K" & SummaryTable).Value = PercentageChange
                
'make loop add extra value at end of new values input into at end of column

                SummaryTable = SummaryTable + 1
            
                TotalVolume = 0
            
                StockOpen = ws.Cells(i + 1, 3).Value
    
                StockClosed = ws.Cells(i, 6).Value
    Else
            
    
            TotalVolume = TotalVolume + ws.Cells(i, 7).Value
      
End If

Next i

Next ws

End Sub

