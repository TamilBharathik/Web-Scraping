import requests
import pandas as pd
import numpy as np
from bs4 import BeautifulSoup
URL = "https://www.worldometers.info/coronavirus/#countries"
html_page = requests.get(URL).text
soup = BeautifulSoup(html_page,'lxml')
get_table = soup.find("table",id="main_table_countries_today")
get_table_data =get_table.tbody.find_all("tr")
dic ={}

for i in range(len(get_table_data )):
    try:
        key = get_table_data[i].find_all("a",href = True)[0].string
    except:
        key = get_table_data[i].find_all("td")[0].string
        
    values = [j.string for j in get_table_data[i].find_all('td')]
    
    dic[key] =values
    
df=pd.DataFrame(dic).iloc[1:,:].T.iloc[:,:9]
df.index_name = "country"

df.to_csv("Corona_live_cases.csv") #data has been saved in the pc
