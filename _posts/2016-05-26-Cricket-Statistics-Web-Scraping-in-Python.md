---
layout: post
title: WebScraping using Python - Cricket Stats
subtitle: Using Python to scrape webpages for all players statistics in a particular year and store the details in a database
bigimg: /img/cricket.png
---

# Cricket Statistics WebScraping using Python

We will be using BeautifulSoup and request for the Web scraping part,a light-weight sqlite database to store the information fetched for further querying and analyis.

This webscraping project involves scraping of Cricket player information such as Bowling and Batting stats of players who featured in the 2015 Cricket World Cup held in Australia.The data fetched would be stored in a database for easier retrieval during analysis.


```python
##main modules
import requests
from bs4 import BeautifulSoup
import re
import csv
import sqlite3
from IPython.display import display,HTML
```

We initialize the url variable with the 2015 world cup series link from espncricinfo website which would be scraped to gain information about players.
Now we get the webpage using the response object created by request.get() method.We can get all the information from this object


```python
url='http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/current/series/509587.html'
page=requests.get(url)
print(type(page))
```

    <class 'requests.models.Response'>
    

Using the respone object created we can check for the current status of the website link with response status code.


```python
print('{} \nWebpage current status code ---> {}'.format(url,page.status_code))
if page.status_code == requests.codes.ok:
    print('>>>The response status code of webpage indicates "ACTIVE" state')
```

    http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/current/series/509587.html 
    Webpage current status code ---> 200
    >>>The response status code of webpage indicates "ACTIVE" state
    

Now we get the content what the webserver returns as a response using the requests get function.
And create a BeautifulSoup object to parse the html response received through the request module get function.


```python
page_html = page.text
soup=BeautifulSoup(page_html,"html.parser")
```
If we inspect element on the webpage and find the country links for 2015 worldcup as below:

```
<li><a name="&amp;lpos=quicklink_Squads" href="/icc-cricket-world-cup-2015/content/squad?object=509587">Squads</a>

<div class="dd_wrap">
 <ul class="subnav_grp">
  <li class="subnav_grpitm">
   <ul class="subnav_tire2">
	<li class="sub_nav_item">...</li>
	 <li class="sub_nav_item">
      <a name="&amp;lpos=quicklink_Australia" href="/icc-cricket-world-cup-2015/content/squad/819741.html">Australia</a>
     </li>
	 <li class="sub_nav_item">
	  <a name="&amp;lpos=quicklink_Bangladesh" href="/icc-cricket-world-cup-2015/content/squad/816431.html">Bangladesh</a>
	 </li>
     <li class="sub_nav_item">
	  <a name="&amp;lpos=quicklink_England" href="/icc-cricket-world-cup-2015/content/squad/812413.html">England</a>
	 </li>
     <li class="sub_nav_item">
	  <a name="&amp;lpos=quicklink_India" href="/icc-cricket-world-cup-2015/content/squad/817409.html">India</a>
	 </li>
     <li class="sub_nav_item">...</li>
     <li class="sub_nav_item">...</li>                        
     <li class="sub_nav_item">...</li>
 </ul>
  </li>
   </ul>
	</div>
    </li>
```

We can see under the li items the a attribute has the links for each country which we would fetch using the soup objects findall method

```python
country_refs=soup.find_all('a',href=re.compile('/icc-cricket-world-cup-2015/content/squad/'))
for link in country_refs:
    print(link)
```

    <a href="/icc-cricket-world-cup-2015/content/squad/814789.html" name="&amp;lpos=quicklink_Afghanistan">Afghanistan</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/819741.html" name="&amp;lpos=quicklink_Australia">Australia</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/816431.html" name="&amp;lpos=quicklink_Bangladesh">Bangladesh</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/812413.html" name="&amp;lpos=quicklink_England">England</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/817409.html" name="&amp;lpos=quicklink_India">India</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/816765.html" name="&amp;lpos=quicklink_Ireland">Ireland</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/818117.html" name="&amp;lpos=quicklink_New Zealand">New Zealand</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/817901.html" name="&amp;lpos=quicklink_Pakistan">Pakistan</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/818887.html" name="&amp;lpos=quicklink_Scotland">Scotland</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/817889.html" name="&amp;lpos=quicklink_South Africa">South Africa</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/817899.html" name="&amp;lpos=quicklink_Sri Lanka">Sri Lanka</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/819825.html" name="&amp;lpos=quicklink_United Arab Emirates">United Arab Emirates</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/819743.html" name="&amp;lpos=quicklink_West Indies">West Indies</a>
    <a href="/icc-cricket-world-cup-2015/content/squad/817903.html" name="&amp;lpos=quicklink_Zimbabwe">Zimbabwe</a>
    

Now we create the database connection object and create necessary tables in database for data inserts.
We create four tables in order to store the data in a normalized manner and reduce data redundancy.This would make future reference and data querying easier and quick.


```python
conn=sqlite3.connect('Cricket.sqlite')
cur=conn.cursor()

cur.execute('''CREATE TABLE IF NOT EXISTS Countries
            (country_id INTEGER PRIMARY KEY,country TEXT)''')

cur.execute('''CREATE TABLE IF NOT EXISTS Players
            (country_id INTEGER,player_id INTEGER UNIQUE,player TEXT)''')

cur.execute('''CREATE TABLE IF NOT EXISTS Batting_Stats
                        (player TEXT,runs INTEGER,sixes INTEGER,fours INTEGER,ducks INTEGER,fifties INTEGER,hundreds INTEGER,balls_faced INTEGER,innings INTEGER)''')
cur.execute('''CREATE TABLE IF NOT EXISTS Bowling_Stats
                        (player TEXT,bowlinnings INTEGER ,overs INTEGER ,runsgiven INTEGER,maidens INTEGER,wickets INTEGER,fourw INTEGER,fivew INTEGER)''')
```




    <sqlite3.Cursor at 0x2e61c5a2d50>



Now we fetch the country name and its respective id from the country_links list and insert into Countries table.


```python
for country_ref in country_refs:
    country=country_ref.text
    country_id=country_ref.get('href').split('/')[-1].split('.')[0]
    cur.execute('INSERT OR IGNORE INTO Countries (country_id,country) VALUES (?,?)',(country_id,country))
```

We select the country_id from Countries table and store in a list 


```python
cur.execute('SELECT country_id FROM Countries')
countryid_list=list()
for row in cur:
    countryid_list.append(row[0])
```

We iterate through the countryid_list and dynamically generate the squad_url link to fetch the player details such as player name,player id and players country id.Again here we are using the soup objects findall method to parse and fetch the information.
Once the information is fetched it is stored in the Players table created previously.


```python
for cid in countryid_list:
    squad_url='http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/squad/'+str(cid)+'.html'
    squad_page=requests.get(squad_url)
    soup1=BeautifulSoup(squad_page.text,"html.parser")
    player_links=soup1.findAll('a', href=re.compile('/icc-cricket-world-cup-2015/content/player/'))
    for i in player_links:
        if i.text:
            player_id=player_id=i.get('href').split('/')[-1].split('.')[0]
            player=i.text.strip()
            cur.execute('INSERT OR IGNORE INTO Players (country_id,player_id,player) VALUES (?,?,?)',(cid,player_id,player))
conn.commit()
```


```python
cur.execute('select a.country_id,a.country,b.player_id,b.player from Countries a,Players b where a.country_id=b.country_id')
play_list=list()
for row in cur:
    play_list.append(row)
play_action=['batting','bowling']
```
Now we iterate over each players action(batting and bowling) and player to fetch the info on both batting and bowling stats accordingly and insert into Batting_Stats and Bowling_Stats table respectively.

We are selecting the year of which we filter the stats of each player and store.Here we are selecting the 2015 year and therefore we are fetching each player stats in international matches played in year 2015.

We can check the link of MS Dhoni as below using filtered stats

<http://stats.espncricinfo.com/ci/engine/player/28081.html?class=2;template=results;type=batting;view=innings;year=2015>

<tr class="data1">  
  <td class="left">filtered</td>  
  <td>20</td>  
  <td>17</td>  
  <td>3</td>  
  <td>640</td> --------->Runs Scored  
  <td>92*</td> --------->High Score  
  <td>45.71</td>--------->Average  
  <td>737</td>  
  <td>86.83</td>  
  <td>0</td>  
  <td>4</td>  
  <td>0</td>  
  <td>50</td>  
  <td>11</td>  
  <td></td>  
 </tr>  

We create the soup object on the url and utilize the findall method for each object to fetch the stats for the selected year.

In order to fetch the stats in order we assign the positions for fetching from td attribute in case of bowling and fielding according to our requirement.

```python
year=2015
for action in play_action:
    for play in play_list:
        pid=play[2]
        country_name=play[1]
        player_name=play[3]
        stats_url='http://stats.espncricinfo.com/ci/engine/player/'+str(pid)+'.html?class=2;template=results;type='+str(action)+';view=innings;year='+str(year)
        stats_page=requests.get(stats_url)
        soup2=BeautifulSoup(stats_page.text,"html.parser")
        stats=soup2.findAll(text='filtered')[0].findParents('tr')[0].findAll("td")
        if action=="bowling":
            check=[3,4,6,5,7,12,13]
        else:
            check=[4,13,12,11,10,9,7,2]
        results=[]
        for i in check:
            try:
                results.append(float(stats[i].get_text()))
            except:
                results.append(0)
        if action=="batting":
            cur.execute('INSERT OR IGNORE INTO Batting_Stats (player,runs,sixes,fours,ducks,fifties,hundreds,balls_faced,innings)VALUES(?,?,?,?,?,?,?,?,?)',(player_name,results[0],results[1],results[2],results[3],results[4],results[5],results[6],results[7]))
        elif action=="bowling":
            cur.execute('INSERT OR IGNORE INTO Bowling_Stats (player,bowlinnings,overs,runsgiven,maidens,wickets,fourw,fivew)VALUES(?,?,?,?,?,?,?,?)',(player_name,results[0],results[1],results[2],results[3],results[4],results[5],results[6]))
conn.commit()
```

Now we have stored the year 2015 batting and bowling stats of each player along with country details in a database which can be used for any kind of analysis.

In this way we could perform granular actions to summarize the player performance and the code can be modified to fetch player statistics for any year.

The full implementation of the scrapper as a single Python program is available in the Github link below:-
- [Python WebScraping Cricket Stats](https://github.com/Ashwin-ks/python-web-scaping-cricket-stats/blob/master/cricket_parser.py)
