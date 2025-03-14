## Building a Cryptocurrency Trade Database using QuestDB with Producer/Consumer Pattern in Julia Notebook.
The Julia notebook sets up a producer/consumer pattern to build a cryptocurrency trade database using QuestDB. The Trade struct is created to store trade data, with a RemoteChannel (from the Distributed package) created to store trades. The WebSocket feed from CoinbasePro is connected, and the relevant fields from incoming JSON objects are parsed and stored as trades in the RemoteChannel. The notebook sets up a QuestDB connection and creates a table with the columns corresponding to the Trade struct. A consumer process reads from the RemoteChannel and writes data to the QuestDB table. The end result is a database of trades that can be queried for further analysis.

Set up parameter:

```
using Sockets
function save_trades_quest(trades)
    cs = connect("docker_host_ip_address", 9009)
    while true
        payload = build_payload(take!(trades))
        write(cs, (payload))
    end
    close(cs)
end
```


## SQL Query for Real-Time Analytics and Aggregations on Trades Table in QuestDB
SQL query written for QuestDB, a relational database management system designed for high-performance querying and real-time analytics. The query selects various columns from a "trades" table, including the timestamp, price, and size, and performs aggregations such as finding the first and last prices and sizes, as well as calculating the percentage change. The WHERE clause filters the results to only include trades for the "ETH-USD" symbol with a timestamp within the last day. The query also samples the results at 1-minute intervals aligned to the calendar. The result set includes columns for the open and close prices, volume, and cosine of the sum of the sizes.

## Grafana Dashboard for Time-Series Data Visualization of ETH (Ethereum) with QuestDB Data Source and SQL Query
JSON dashboard file for Grafana, a platform for visualizing and analyzing data from various sources. The dashboard has an ID of 2 and displays time-series data for ETH (Ethereum). The data is retrieved from a QuestDB data source using a SQL query that retrieves information such as the opening and closing prices, volume, and percentage change for the past day. The dashboard contains options for tooltip display and legend placement, and field configurations for customizing the visualization of the data. The targets section specifies the data source and SQL query to be executed to retrieve the data.
