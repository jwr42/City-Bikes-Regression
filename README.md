## Helsinki City Bike Linear Regression Model

Predictive linear regression model constructed in Power BI that uses the outside air temperature to predict the number of journeys taken on the Helsinki bike system, in the Helsinki and Espoo metropolitan areas of Finland.

### Features

- Applied Power Query Editor steps to aggregate the data from the source to reduce the size of the imported data model
- DAX Script to create two tables for the training data and testing data, using a 70/30 split
- Data Visualisations of the training and testing data
- Power BI What If parameter for user interaction with the final linear regression model

### Data Source

https://www.kaggle.com/datasets/geometrein/helsinki-city-bikes

### Dashboard Screenshot

<img src="screenshots/dashboard.png" alt="dashboard" width="800"/>

### Code Extracts

```sql
let
    Source = Csv.Document(File.Contents("C:\Users\jonat\OneDrive\Code\Power BI - Helsinki City Bikes\data\database.csv"),[Delimiter=",", Columns=14, Encoding=65001, QuoteStyle=QuoteStyle.None]),
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]),
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{{"departure", type datetime}, {"return", type datetime}, {"departure_id", Int64.Type}, {"departure_name", type text}, {"return_id", Int64.Type}, {"return_name", type text}, {"distance (m)", type number}, {"duration (sec.)", Int64.Type}, {"avg_speed (km/h)", type number}, {"departure_latitude", type number}, {"departure_longitude", type number}, {"return_latitude", type number}, {"return_longitude", type number}, {"Air temperature (degC)", type number}}),
    #"Inserted Date" = Table.AddColumn(#"Changed Type", "Date", each DateTime.Date([departure]), type date),
    #"Reordered Columns" = Table.ReorderColumns(#"Inserted Date",{"Date", "departure", "return", "departure_id", "departure_name", "return_id", "return_name", "distance (m)", "duration (sec.)", "avg_speed (km/h)", "departure_latitude", "departure_longitude", "return_latitude", "return_longitude", "Air temperature (degC)"}),
    #"Grouped Rows" = Table.Group(#"Reordered Columns", {"Date"}, {{"Rides", each Table.RowCount(_), Int64.Type}, {"AvgAirTemp", each List.Average([#"Air temperature (degC)"]), type nullable number}}),
    #"Sorted Rows" = Table.Sort(#"Grouped Rows",{{"Date", Order.Descending}}),
    #"Filtered Rows" = Table.SelectRows(#"Sorted Rows", each [Date] > #date(2019, 1, 1))
in
    #"Filtered Rows"
```



