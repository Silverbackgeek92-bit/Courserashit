# Courserashit
Stupi assignment

import yfinance as yf

import pandas as pd

import requests

from bs4 import BeautifulSoup

import plotly.graph_objects as go

from plotly.subplots import make_subplots

​

def make_graph(stock_data, revenue_data, stock):

    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing=0.3)

    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data.Date, infer_datetime_format=True), y=stock_data.Close.astype("float"), name="Share Price"), row=1, col=1)

    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data.Date, infer_datetime_format=True), y=revenue_data.Revenue.astype("float"), name="Revenue"), row=2, col=1)

    fig.update_xaxes(title_text="Date", row=1, col=1)

    fig.update_xaxes(title_text="Date", row=2, col=1)

    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)

    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)

    fig.update_layout(showlegend=False, height=900, title=stock, xaxis_rangeslider_visible=True)

    fig.show()

​

# Fetch GameStop stock data

GameStop = yf.Ticker("GME")

GameStop_data = GameStop.history(period="max")

GameStop_data.reset_index(inplace=True)

​

# Fetch revenue data for GameStop

url_gamestop = "https://www.macrotrends.net/stocks/charts/GME/gamestop/revenue"

html_data_gamestop = requests.get(url_gamestop).text

soup_gamestop = BeautifulSoup(html_data_gamestop, "html5lib")

table_index_gamestop = None

tables_gamestop = soup_gamestop.find_all('table')

​

# Find the correct table for GameStop revenue data

for index, table in enumerate(tables_gamestop):

    if "GameStop Quarterly Revenue" in str(table):

        table_index_gamestop = index

        break

​

# Check if the revenue table was found

if table_index_gamestop is not None:

    GameStop_revenue = pd.DataFrame(columns=["Date", "Revenue"])

    for row in tables_gamestop[table_index_gamestop].tbody.find_all("tr"):

        col = row.find_all("td")

        if col:

            Date = col[0].text

            Revenue = col[1].text.replace("$", "").replace(",", "")

            GameStop_revenue = GameStop_revenue.append({"Date": Date, "Revenue": Revenue}, ignore_index=True)

    # Create the graph if revenue data is available

    make_graph(GameStop_data, GameStop_revenue, "GameStop")

else:

    print("Revenue table not found for GameStop.")

​

# Display the stock and revenue data for GameStop

print("GameStop Stock Data:")

print(GameStop_data.head())

print("\nGameStop Revenue Data:")

if "GameStop_revenue" in locals():

    print(GameStop_revenue.head())

else:

    print("Revenue data for GameStop is not available.")

​

Revenue table not found for GameStop.
GameStop Stock Data:
                       Date      Open      High       Low     Close    Volume  \
0 2002-02-13 00:00:00-05:00  1.620129  1.693350  1.603296  1.691667  76216000   
1 2002-02-14 00:00:00-05:00  1.712707  1.716073  1.670626  1.683250  11021600   
2 2002-02-15 00:00:00-05:00  1.683250  1.687458  1.658002  1.674834   8389600   
3 2002-02-19 00:00:00-05:00  1.666418  1.666418  1.578047  1.607504   7410400   
4 2002-02-20 00:00:00-05:00  1.615920  1.662210  1.603296  1.662210   6892800   

   Dividends  Stock Splits  
0        0.0           0.0  
1        0.0           0.0  
2        0.0           0.0  
3        0.0           0.0  
4        0.0           0.0  

GameStop Revenue Data:
Revenue data for GameStop is not available.




import yfinance as yf

import pandas as pd

import requests

from bs4 import BeautifulSoup

import plotly.graph_objects as go

from plotly.subplots import make_subplots

​

def make_graph(stock_data, revenue_data, stock):

    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing=0.3)

    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data.Date, infer_datetime_format=True), y=stock_data.Close.astype("float"), name="Share Price"), row=1, col=1)

    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data.Date, infer_datetime_format=True), y=revenue_data.Revenue.astype("float"), name="Revenue"), row=2, col=1)

    fig.update_xaxes(title_text="Date", row=1, col=1)

    fig.update_xaxes(title_text="Date", row=2, col=1)

    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)

    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)

    fig.update_layout(showlegend=False, height=900, title=stock, xaxis_rangeslider_visible=True)

    fig.show()

​

# Fetch GameStop stock data

GameStop = yf.Ticker("GME")

GameStop_data = GameStop.history(period="max")

GameStop_data.reset_index(inplace=True)

​

# Fetch revenue data for GameStop

url_gamestop = "https://www.macrotrends.net/stocks/charts/GME/gamestop/revenue"

html_data_gamestop = requests.get(url_gamestop).text

soup_gamestop = BeautifulSoup(html_data_gamestop, "html5lib")

table_index_gamestop = None

tables_gamestop = soup_gamestop.find_all('table')

​

# Find the correct table for GameStop revenue data

for index, table in enumerate(tables_gamestop):

    if "GameStop Quarterly Revenue" in str(table):

        table_index_gamestop = index

        break

​

# Check if the revenue table was found

if table_index_gamestop is not None:

    GameStop_revenue = pd.DataFrame(columns=["Date", "Revenue"])

    for row in tables_gamestop[table_index_gamestop].tbody.find_all("tr"):

        col = row.find_all("td")

        if col:

            Date = col[0].text

            Revenue = col[1].text.replace("$", "").replace(",", "")

            GameStop_revenue = GameStop_revenue.append({"Date": Date, "Revenue": Revenue}, ignore_index=True)

    # Create the graph if revenue data is available

    make_graph(GameStop_data, GameStop_revenue, "GameStop")

else:

    print("Revenue table not found for GameStop.")

​

# Display the stock and revenue data for GameStop

print("GameStop Stock Data:")

print(GameStop_data.head())

print("\nGameStop Revenue Data:")

if "GameStop_revenue" in locals():

    print(GameStop_revenue.head())

else:

    print("Revenue data for GameStop is not available.")

​

Revenue table not found for GameStop.
GameStop Stock Data:
                       Date      Open      High       Low     Close    Volume  \
0 2002-02-13 00:00:00-05:00  1.620129  1.693350  1.603296  1.691667  76216000   
1 2002-02-14 00:00:00-05:00  1.712707  1.716073  1.670626  1.683250  11021600   
2 2002-02-15 00:00:00-05:00  1.683250  1.687458  1.658002  1.674834   8389600   
3 2002-02-19 00:00:00-05:00  1.666418  1.666418  1.578047  1.607504   7410400   
4 2002-02-20 00:00:00-05:00  1.615920  1.662210  1.603296  1.662210   6892800   

   Dividends  Stock Splits  
0        0.0           0.0  
1        0.0           0.0  
2        0.0           0.0  
3        0.0           0.0  
4        0.0           0.0  

GameStop Revenue Data:
Revenue data for GameStop is not available.
