# Billboard's 50 Best Afrobeats Songs of the Past Decade
**Bayowa Onabajo**
**12/08/2025**

### Introduction: 
Over the past decade, Afrobeats has evolved from a regional West African sound into a globally influential musical movement. Artists from across Africa,particularly West Africa,have achieved unprecedented international visibility through chart performance, cross-genre collaborations, and cultural impact. Despite this global recognition, questions remain about which songs and artists have most consistently defined the afrobeat genre’s rise.

This explores Billboard’s curated list of the 50 Best Afrobeats Songs to examine which tracks, artists, and time periods have played a central role in shaping Afrobeats’ global trajectory. By extracting and analyzing this dataset, the project combines data-collection with cultural inquiry. Beyond the analytical motivation, this work is also personally meaningful: as a Nigerian, Afrobeats represents both a cultural identity and a living archive of contemporary African creativity.

### Objective: 
- Extraction of top 50 afrobeats songs from the billboard web-page.  
- Clean and structure the extracted data for analysis.  
- Perform exploratory data analysis and basic visualizations.  
- Create a fully documented Jupyter Notebook file (".ipynb").
- Create a Markdown summary of findings.
- Create an optional CSV file for downstream analysis in future projects.

### Data Source
Billboard – Best Afrobeats Songs  
https://www.billboard.com/lists/best-afrobeats-songs/

### Libraries
- **requests**: retrieve HTML content from Billboard
- **BeautifulSoup**: parse and extract song data from the webpage
- **pandas**: clean, structure, and analyze the dataset
- **time**: to control request timing


```python
# Load libraries
import pandas as pd
import numpy as np

#install packages if needed
!pip install seaborn
!pip install wordcloud 

# scraping
import requests
from bs4 import BeautifulSoup
import time

# Visualization
import matplotlib.pyplot as plt
import seaborn as sns
from wordcloud import WordCloud  

# System
import os
from datetime import datetime

print("Libraries imported!")
```

    Requirement already satisfied: seaborn in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (0.13.2)
    Requirement already satisfied: numpy!=1.24.0,>=1.20 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from seaborn) (2.3.2)
    Requirement already satisfied: pandas>=1.2 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from seaborn) (2.3.2)
    Requirement already satisfied: matplotlib!=3.6.1,>=3.4 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from seaborn) (3.10.5)
    Requirement already satisfied: contourpy>=1.0.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (1.3.3)
    Requirement already satisfied: cycler>=0.10 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (4.59.1)
    Requirement already satisfied: kiwisolver>=1.3.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (1.4.9)
    Requirement already satisfied: packaging>=20.0 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (24.1)
    Requirement already satisfied: pillow>=8 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (11.3.0)
    Requirement already satisfied: pyparsing>=2.3.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (3.2.3)
    Requirement already satisfied: python-dateutil>=2.7 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (2.9.0.post0)
    Requirement already satisfied: pytz>=2020.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from pandas>=1.2->seaborn) (2025.2)
    Requirement already satisfied: tzdata>=2022.7 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from pandas>=1.2->seaborn) (2025.2)
    Requirement already satisfied: six>=1.5 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from python-dateutil>=2.7->matplotlib!=3.6.1,>=3.4->seaborn) (1.16.0)
    Requirement already satisfied: wordcloud in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (1.9.4)
    Requirement already satisfied: numpy>=1.6.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from wordcloud) (2.3.2)
    Requirement already satisfied: pillow in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from wordcloud) (11.3.0)
    Requirement already satisfied: matplotlib in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from wordcloud) (3.10.5)
    Requirement already satisfied: contourpy>=1.0.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (1.3.3)
    Requirement already satisfied: cycler>=0.10 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (4.59.1)
    Requirement already satisfied: kiwisolver>=1.3.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (1.4.9)
    Requirement already satisfied: packaging>=20.0 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (24.1)
    Requirement already satisfied: pyparsing>=2.3.1 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (3.2.3)
    Requirement already satisfied: python-dateutil>=2.7 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from matplotlib->wordcloud) (2.9.0.post0)
    Requirement already satisfied: six>=1.5 in /opt/anaconda3/envs/jupyterlab/lib/python3.11/site-packages (from python-dateutil>=2.7->matplotlib->wordcloud) (1.16.0)
    Libraries imported!



```python
url = "https://www.billboard.com/lists/best-afrobeats-songs/"

headers = {
    "User-Agent": "Mozilla/5.0"
}

response = requests.get(url, headers=headers)

response.status_code

```




    200




```python
soup = BeautifulSoup(response.text, "html.parser")

```


```python
soup.prettify()[:1000]

```




    '<!DOCTYPE html>\n<!--[if IE 6]>\n<html id="ie6" lang="en-US">\n<![endif]-->\n<!--[if IE 7]>\n<html id="ie7" lang="en-US">\n<![endif]-->\n<!--[if IE 8]>\n<html id="ie8" lang="en-US">\n<![endif]-->\n<!--[if !(IE 6) | !(IE 7) | !(IE 8) ]><!-->\n<html lang="en-US">\n <!--<![endif]-->\n <head>\n  <meta charset="utf-8"/>\n  <meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible"/>\n  <meta content="#ffffff" name="theme-color"/>\n  <meta content="width=device-width, initial-scale=1.0" name="viewport">\n   <!-- Add to home screen for iOS -->\n   <meta content="black-translucent" name="apple-mobile-web-app-status-bar-style"/>\n   <link href="https://www.billboard.com/wp-content/themes/vip/pmc-billboard-2021/assets/app/icons/apple-touch-icon.png" rel="apple-touch-icon" sizes="180x180"/>\n   <!-- Tile icons for Windows -->\n   <meta content="https://www.billboard.com/wp-content/themes/vip/pmc-billboard-2021/assets/app/browserconfig.xml" name="msapplication-config"/>\n   <meta content="https://www.billboard.com/wp'




```python
import json
import re
scripts = soup.find_all("script")

len(scripts)

```




    134




```python
# Examine contents of few script tags 
for i, script in enumerate(scripts[:10]):
    print(f"--- Script {i} ---")
    print(script.text[:300])

```

    --- Script 0 ---
    
    			window.dataLayer = window.dataLayer || [];
    			function gtag(){dataLayer.push(arguments);}
    
    			window.pmc_google_consent_mode = {
    				init: function(opts = {}) {
    					/** @type string[] */
    					const consentGroups = window.pmc_onetrust_helpers?.getActiveConsentGroups?.() || [];
    
    					/**
    					 * 
    --- Script 1 ---
    
    /* <![CDATA[ */
    var pmc_meta = {"lob":"billboard","lob_genre":"Music","page-type":"single-pmc_list","env":"desktop","primary-category":"Music","primary-vertical":"","vertical":"","category":["Music","Music News"],"tag":["African Music"],"author":"","logged-in":"","subscriber-type":"","country":"us"
    --- Script 2 ---
    
    			(function(d,w){
    				var i, parts, name, c, rdecode = /(%[0-9A-Z]{2})+/g, rspace = /\+/g, ac = (d ? d.split('; ') : []);
    				for(w.pmc_cookies = {}, i = 0; i < ac.length; i++) {
    					parts = ac[i].split('='), name = parts[0].replace(rdecode, decodeURIComponent), c = parts.slice(1).join('=');
    				
    --- Script 3 ---
    
    			if ( window.hasOwnProperty( 'pmc_meta' ) ) {
    				window.dataLayer = window.dataLayer || [];
    				window.dataLayer.push( pmc_meta );
    			}
    		
    --- Script 4 ---
    
    			window.pmc_is_adblocked = false;
    		
    --- Script 5 ---
    
    
    --- Script 6 ---
    
    --- Script 7 ---
    
    --- Script 8 ---
    
    			var pmc_do_analytics_pagecount = true;		//flag to allow analytics code to count a page view
    			var pmc_common_urls = {
    				parent_theme_uri: 'https://www.billboard.com/wp-content/themes/vip/pmc-core-v2/',
    				current_theme_uri: 'https://www.billboard.com/wp-content/themes/vip/pmc-billboard-2021/
    --- Script 9 ---
    
    			var pmc_ga_dimensions = {"dimension40":"single-pmc_list","dimension42":"1236023961","dimension43":"heran-mamo|blossom-maduafokwa|wongo-okon|nicolas-tyrell-scott|wale-oloworekende","dimension44":"music|music-news","dimension45":"african-music","dimension47":"music","dimension49":"2025","dimension



```python
json_scripts = soup.find_all("script", type="application/ld+json")
len(json_scripts)

```




    4




```python
json_scripts[0].string[:500]

```




    '\n\t\t\t\t{\n    "@context": "https://schema.org",\n    "@type": "Organization",\n    "url": "https://www.billboard.com",\n    "name": "Billboard",\n    "logo": "https://www.billboard.com/wp-content/themes/vip/pmc-billboard-2021/assets/build/svg/billboard.svg"\n}\n\t\t\t'




```python
for i, script in enumerate(json_scripts):
    print(f"\n--- JSON Script {i} ---\n")
    print(script.string[:600])

```

    
    --- JSON Script 0 ---
    
    
    				{
        "@context": "https://schema.org",
        "@type": "Organization",
        "url": "https://www.billboard.com",
        "name": "Billboard",
        "logo": "https://www.billboard.com/wp-content/themes/vip/pmc-billboard-2021/assets/build/svg/billboard.svg"
    }
    			
    
    --- JSON Script 1 ---
    
    {"@context":"https:\/\/schema.org","@type":"NewsArticle","headline":"The 50 Best Afrobeats Songs of All Time: Full Staff List","url":"http:\/\/www.billboard.com\/lists\/best-afrobeats-songs\/","mainEntityOfPage":{"@type":"WebPage","@id":"http:\/\/www.billboard.com\/lists\/best-afrobeats-songs\/"},"thumbnailUrl":"https:\/\/www.billboard.com\/wp-content\/uploads\/2025\/07\/50-Best-Afrobeats-Songs-of-All-Time-List-HERO-Staff-Picks-2025-billboard-1548.jpg?w=150&h=150&crop=1","image":{"@type":"ImageObject","url":"https:\/\/www.billboard.com\/wp-content\/uploads\/2025\/07\/50-Best-Afrobeats-Songs-of
    
    --- JSON Script 2 ---
    
    
    			{"@context":"https:\/\/schema.org","@type":"BreadcrumbList","itemListElement":[{"@type":"ListItem","position":1,"name":"Music","item":"https:\/\/www.billboard.com\/c\/music\/"},{"@type":"ListItem","position":2,"name":"Music News","item":"https:\/\/www.billboard.com\/c\/music\/music-news\/"}]}		
    
    --- JSON Script 3 ---
    
    
    {
        "@context": "http://schema.org",
        "@type": "NewsArticle",
        "url": "https://www.billboard.com/lists/best-afrobeats-songs/",
        "name": "Billboard",
        "mainEntityOfPage": {
            "@type": "WebPage",
            "@id": "https://www.billboard.com/lists/best-afrobeats-songs/"
        },
        "headline": "Best Afrobeats Songs: 50 African Music Classics (Critics’ Picks)",
        "datePublished": "2025-08-19T17:10:00+00:00",
        "dateModified": "2025-08-19T17:10:00+00:00",
        "articleBody": "We're rolling out the 50 tracks that are the most foundational, influential and popular within Afrobeats



```python
h3_tags = soup.find_all("h3")
len(h3_tags)

```




    33




```python
for h in h3_tags[:10]:
    print(h.get_text(strip=True))

```

    Want to know what everyone in the music business is talking about?
    Get in the know on
    Trending
    How ‘KPop Demon Hunters’ Changed the Game For Netflix
    Lady Gaga Abruptly Stops Mayhem Ball Show Mid-Performance After Dancer Falls Off Stage in Australia
    Debby Ryan Welcomes Baby With Twenty One Pilots’ Josh Dun
    Riley Green & Ella Langley Join Blake Shelton & Gwen Stefani for History Atop Country Airplay Chart
    The Daily
    Jennifer Lawrence, Josh Hutcherson to Return for 'Hunger Games: Sunrise on the Reaping'
    Jewel’s Super-Rare Bikini Photos Show She’s Living Her Best Life ‘Before More Winter Snow'



```python
li_tags = soup.find_all("li")
len(li_tags)

```




    237




```python
for li in li_tags[:20]:
    print(li.get_text(strip=True))

```

    Charts
    Music
    Video
    Crossword
    Awards
    Business
    Manage Account
    Log Out
    Charts
    Music
    Video
    Crossword
    Awards
    Business
    Manage Account
    Log Out
    Hot 100
    Chart Beat
    Year End Charts
    Shop



```python
articles = soup.find_all("article")
len(articles)

```




    51




```python
for a in articles[:10]:
    print(a.get_text(strip=True)[:300])
    print("-" * 40)

```

    Over the last decade, Afrobeats has made significant inroads in the global music industry, from invitations to conquer the biggest stages in the world to cross-cultural collaborations with Western superstars like Beyoncé, Drake and Ed Sheeran. And it’s earned institutional recognition.Billboardlaunc
    ----------------------------------------
    50. Weird MC, “Ijoya” (2006)To put it simply, Weird MC was radical. She strode onto the scene with baggy pants, a shaved head and a hip-hop-inspired bravado – incredibly novel for women who were already the minority in Nigerian music. On the Don Jazzy and JJC Skillz-produced “Ijoya,” she proved hers
    ----------------------------------------
    49. Nonso Amadi, “Tonight” (2016)Soulfully tinged with R&B tendencies, Nonso Amandi’s breakout hit “Tonight” bathes in an array of genres and thus articulates the breadth and depth of Afrobeats in the middle of the 2010s. The song not only helped spearhead Afro-R&B — a pocket in which artists like T
    ----------------------------------------
    48. Timaya, “Dem Mama” (2005)Image Credit: Courtesy PhotoThere is no Afrobeats without the conscious music of its earliest stars like Timaya. On his 2005 breakout single “Dem Mama,” the Port Harcourt-born singer decried the heavy-handedness of the Nigerian government and its armed forces. Specifical
    ----------------------------------------
    47. Jazzman Olofin feat. Adewale Ayuba, “Raise Da Roof” (2004)Long before Afrobeats fully emerged in the early 2000s, indigenous genres like fuji, apala and highlife inspired Nigerian music. “Raise Da Roof” paid homage to that sonic lineage by melding hip-hop and fuji for a futuristic take on Afrobe
    ----------------------------------------
    46. Victony & Tempoe, “Soweto” (2022)“Soweto” emerged as a semi-sleeper hit from Victony’s 2022 EPOutlaw. Masterminded by the ever-talented Tempoe, “Soweto” is led by an addictive guitar riff and Victony’s suggestive pen. The song eventually blew up on social media with smooth TikTok dance challenge
    ----------------------------------------
    45. Olu Maintain, “Yahooze” (2007)With minimal institutional support accessible in the early days of Afrobeats, fraudsters – or “yahoo boys,” as Nigerians affectionately call them – were a critical source of much-needed cash. While Olu “maintains” that the song is simply about a young talent coming 
    ----------------------------------------
    44. Ice Prince feat. Brymo, “Oleku” (2010)Hip-hop is undoubtedly an integral part of Afrobeats’ DNA, and no label underscored its importance better than Chocolate City. Following the early-2000s success of brothers M.I Abaga and Jesse Jagz, Ice Prince was touted as a star-in-waiting after a series o
    ----------------------------------------
    43. Kizz Daniel feat. Tekno, “Buga” (2022)Since breaking into the Afrobeats scene in 2014 with “Woju,” Kizz Daniel has proven his talents pass the tests of time on multiple occasions. Nearly a decade after “Woju,” Kizz Daniel came roaring back with the Tekno-assisted “Buga,” making Afrobeats lovers 
    ----------------------------------------
    42. Libianca, “People” (2022)The Cameroonian American singer-songwriter pours her raw melancholy and a stirring melodic blend of Afrobeats and R&B into her sobering breakout hit “People.” The forthright opening lines serve as a wake-up call for those who can’t see what their friends are really facin
    ----------------------------------------



```python
song_articles = []

for a in articles:
    text = a.get_text(" ", strip=True)
    if any(str(i) in text[:20] for i in range(1, 51)):
        song_articles.append(text)

len(song_articles)

```




    50




```python
song_articles[:5]

```




    ['50. Weird MC, “Ijoya” (2006) To put it simply, Weird MC was radical. She strode onto the scene with baggy pants, a shaved head and a hip-hop-inspired bravado –\xa0incredibly novel for women who were already the minority in Nigerian music. On the Don Jazzy and JJC Skillz-produced “Ijoya,” she proved herself to be ahead of her time not only where aesthetics were concerned, but also in craft. Her slick Yoruba lyricism and rapid-fire delivery, accentuated now and again by talking drums, make “Ijoya” an ageless dance hit, while the song’s inventive visuals made her the first-ever Afrobeats artist to release an animated music video. – BLOSSOM MADUAFOKWA',
     '49. Nonso Amadi, “Tonight” (2016) Soulfully tinged with R&B tendencies, Nonso Amandi’s breakout hit “Tonight” bathes in an array of genres and thus articulates the breadth and depth of Afrobeats in the middle of the 2010s.\xa0The song not only helped spearhead Afro-R&B — a pocket in which artists like Tems comfortably sit today — but also builds on the act of yearning, which Afrobeats is well known for. It adds to the canon of male Afrobeats acts shedding their ego and leaning into vulnerability. And with “Tonight,” the Nigeria-born, Canada-based singer scored a top 10 on Nigeria’s now-defunct Playdata airplay chart. – NICOLAS-TYRELL SCOTT',
     '48. Timaya, “Dem Mama” (2005) Image Credit: Courtesy Photo There is no Afrobeats without the conscious music of its earliest stars like Timaya. On his 2005 breakout single “Dem Mama,” the Port Harcourt-born singer decried the heavy-handedness of the Nigerian government and its armed forces. Specifically referencing the 1999 massacre in his hometown, Odi, located in Bayelsa State, he wove a haunting tale of the military’s brutal attack that cost 30 lives . Critiquing a democracy that was in its infancy, just as Afrobeats was taking form, “Dem Mama” was both an unflinching portrait of its time and a social justice anthem to rally around. – WALE OLOWOREKENDE',
     '47. Jazzman Olofin feat. Adewale Ayuba, “Raise Da Roof” (2004) Long before Afrobeats fully emerged in the early 2000s, indigenous genres like fuji, apala and highlife inspired Nigerian music. “Raise Da Roof” paid homage to that sonic lineage by melding hip-hop and fuji for a futuristic take on Afrobeats, crafting a seminal hit that’s still a party favorite to this day. OJB Jezreel’s skittering production and the synergy between Jazzman Olofin and fuji icon Adewale Ayuba gives the song a playful edge that has provided a template for the future takeover of the genre by fuji-adjacent stars like Asake and Seyi Vibez. – W. OLOWOREKENDE',
     '46. Victony & Tempoe, “Soweto” (2022) “Soweto” emerged as a semi-sleeper hit from Victony’s 2022 EP Outlaw . Masterminded by the ever-talented Tempoe, “Soweto” is led by an addictive guitar riff and Victony’s suggestive pen. The song eventually blew up on social media with smooth TikTok dance challenges and resulted in a four-week No. 1 stint on the UK Afrobeats chart, and a top 10 entry on the U.S. Afrobeats Songs and TurnTable Top 50 (which has since rebranded as the Official Nigeria Top 100), thanks to remixes with Don Toliver and Rema as well as Omah Lay. – B.M.']




```python
import re
import pandas as pd

ranks = []
artists = []
songs = []
years = []
descriptions = []

```


```python
ranks, artists, songs, years, descriptions

```




    ([], [], [], [], [])




```python
for entry in song_articles:
    
    # Rank
    rank_match = re.match(r"(\d+)\.", entry)
    rank = int(rank_match.group(1)) if rank_match else None

    # Year
    year_match = re.search(r"\((\d{4})\)", entry)
    year = int(year_match.group(1)) if year_match else None

    # divide main header from description
    header, *desc = entry.split(")", 1)
    description = desc[0].strip() if desc else ""

    # Extraction of artist and song
    header_clean = header.split(".", 1)[1].strip()
    artist_song = header_clean.split(",", 1)

    artist = artist_song[0].strip()
    song = artist_song[1].replace("“", "").replace("”", "").strip() if len(artist_song) > 1 else None

    # Append
    ranks.append(rank)
    artists.append(artist)
    songs.append(song)
    years.append(year)
    descriptions.append(description)

```


```python
len(song_articles)

```




    50




```python
import re

ranks = []
artists = []
songs = []
years = []
descriptions = []

pattern = re.compile(
    r"(\d+)\.\s*(.*?)\,\s*“(.*?)”\s*\((\d{4})\)\s*(.*)"
)

for text in song_articles:
    match = pattern.search(text)
    if match:
        ranks.append(match.group(1))
        artists.append(match.group(2))
        songs.append(match.group(3))
        years.append(match.group(4))
        descriptions.append(match.group(5).strip())

```


```python
len(ranks), len(artists), len(songs), len(years)

```




    (50, 50, 50, 50)




```python
len(ranks), len(artists), len(songs), len(years), len(descriptions)

```




    (50, 50, 50, 50, 50)




```python
df_afrobeats50 = pd.DataFrame({
    "Rank": ranks,
    "Artist": artists,
    "Song Title": songs,
    "Year": years,
    "Description": descriptions
})

df_afrobeats50.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Artist</th>
      <th>Song Title</th>
      <th>Year</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>50</td>
      <td>Weird MC</td>
      <td>Ijoya</td>
      <td>2006</td>
      <td>To put it simply, Weird MC was radical. She st...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>49</td>
      <td>Nonso Amadi</td>
      <td>Tonight</td>
      <td>2016</td>
      <td>Soulfully tinged with R&amp;B tendencies, Nonso Am...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>48</td>
      <td>Timaya</td>
      <td>Dem Mama</td>
      <td>2005</td>
      <td>Image Credit: Courtesy Photo There is no Afrob...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>47</td>
      <td>Jazzman Olofin feat. Adewale Ayuba</td>
      <td>Raise Da Roof</td>
      <td>2004</td>
      <td>Long before Afrobeats fully emerged in the ear...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>46</td>
      <td>Victony &amp; Tempoe</td>
      <td>Soweto</td>
      <td>2022</td>
      <td>“Soweto” emerged as a semi-sleeper hit from Vi...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>45</td>
      <td>Olu Maintain</td>
      <td>Yahooze</td>
      <td>2007</td>
      <td>With minimal institutional support accessible ...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>44</td>
      <td>Ice Prince feat. Brymo</td>
      <td>Oleku</td>
      <td>2010</td>
      <td>Hip-hop is undoubtedly an integral part of Afr...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>43</td>
      <td>Kizz Daniel feat. Tekno</td>
      <td>Buga</td>
      <td>2022</td>
      <td>Since breaking into the Afrobeats scene in 201...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>42</td>
      <td>Libianca</td>
      <td>People</td>
      <td>2022</td>
      <td>The Cameroonian American singer-songwriter pou...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>41</td>
      <td>Asake</td>
      <td>Peace Be Unto You (PBUY)</td>
      <td>2022</td>
      <td>Image Credit: Courtesy Photo By the time Asake...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Remove "image credit" from Description column
df_afrobeats50["Description"] = df_afrobeats50["Description"].str.replace(
    r"Image Credit:.*?(?=[A-Z])", "", regex=True
)

```


```python
#Converting Rank and Year columns to numeric
df_afrobeats50["Rank"] = df_afrobeats50["Rank"].astype(int)
df_afrobeats50["Year"] = df_afrobeats50["Year"].astype(int)

```


```python
df_afrobeats50 = df_afrobeats50.sort_values("Rank")
df_afrobeats50.head(10)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Artist</th>
      <th>Song Title</th>
      <th>Year</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>49</th>
      <td>1</td>
      <td>2Baba</td>
      <td>African Queen</td>
      <td>2004</td>
      <td>When the Nigerian boy band Plantashun Boiz dis...</td>
    </tr>
    <tr>
      <th>48</th>
      <td>2</td>
      <td>Wizkid</td>
      <td>Ojuelegba</td>
      <td>2014</td>
      <td>Courtesy Photo In the aftermath of early caree...</td>
    </tr>
    <tr>
      <th>47</th>
      <td>3</td>
      <td>Flavour</td>
      <td>Nwa Baby (Ashawo Remix)</td>
      <td>2011</td>
      <td>While the world is much more acquainted with t...</td>
    </tr>
    <tr>
      <th>46</th>
      <td>4</td>
      <td>Rema</td>
      <td>Calm Down</td>
      <td>2022</td>
      <td>What sounds on its surface like a mid-tempo lo...</td>
    </tr>
    <tr>
      <th>45</th>
      <td>5</td>
      <td>Wizkid feat. Tems</td>
      <td>Essence</td>
      <td>2020</td>
      <td>“Essence” is a masterfully crafted hit, each c...</td>
    </tr>
    <tr>
      <th>44</th>
      <td>6</td>
      <td>CKay</td>
      <td>Love Nwantiti</td>
      <td>2019</td>
      <td>CKay’s “Love Nwantiti” is instantly sultry, wi...</td>
    </tr>
    <tr>
      <th>43</th>
      <td>7</td>
      <td>D’banj</td>
      <td>Oliver Twist</td>
      <td>2011</td>
      <td>Christie Goodwin/Redferns/Getty Images Before ...</td>
    </tr>
    <tr>
      <th>42</th>
      <td>8</td>
      <td>Davido</td>
      <td>Fall</td>
      <td>2017</td>
      <td>Davido showers his lucky lady with dollars and...</td>
    </tr>
    <tr>
      <th>41</th>
      <td>9</td>
      <td>Burna Boy</td>
      <td>Ye</td>
      <td>2018</td>
      <td>A notorious and undisputed Burna Boy classic, ...</td>
    </tr>
    <tr>
      <th>40</th>
      <td>10</td>
      <td>P-Square</td>
      <td>Chop My Money (Remix)</td>
      <td>2012</td>
      <td>At the time the “Chop My Money (Remix)” was re...</td>
    </tr>
  </tbody>
</table>
</div>




```python
#CSV file
df_afrobeats50.to_csv("top_50_afrobeats_billboard.csv", index=False)

```


```python
df_afrobeats50["Year"].value_counts().sort_index()

```




    Year
    2003    1
    2004    2
    2005    1
    2006    1
    2007    1
    2008    3
    2009    1
    2010    1
    2011    4
    2012    1
    2013    4
    2014    4
    2015    2
    2016    6
    2017    1
    2018    2
    2019    2
    2020    2
    2021    2
    2022    8
    2023    1
    Name: count, dtype: int64




```python
import matplotlib.pyplot as plt

df_afrobeats50["Year"].value_counts().sort_index().plot(kind="bar", figsize=(10,5))
plt.title("Top Afrobeats Songs in the past Decade by Year (Billboard)")
plt.xlabel("Year")
plt.ylabel("Number of Songs")
plt.show()

```


    
![png](output_31_0.png)
    


### Temporal Trends:
The bar chart illustrates the number of top-ranked songs by year, showing a relatively sparse distribution in the early 2000s, followed by steady growth after 2010. The pronounced increase around 2011 and the strong representation in 2022 reflect key moments when Afrobeats gained heightened international exposure. These spikes likely correspond to factors such as increased global chart penetration, the advent of social media, viral digital distribution, and broader Afrobeats appeal.


```python
df_afrobeats50["Artist"].value_counts().sort_index()
```




    Artist
    2Baba                                 1
    9ice                                  1
    Afro B                                1
    Amaarae & MOLIY                       1
    Asake                                 1
    Ayra Starr                            1
    Burna Boy                             2
    Burna Boy feat. Zlatan                1
    CKay                                  1
    Davido                                2
    Davido feat. Musa Keys                1
    D’banj                                2
    Fireboy DML                           1
    Flavour                               1
    Fuse ODG feat. Itz Tiffany            1
    Ice Prince feat. Brymo                1
    Jazzman Olofin feat. Adewale Ayuba    1
    Kizz Daniel feat. Tekno               1
    Libianca                              1
    Lojay & Sarz                          1
    Maleek Berry                          1
    Mr. Eazi & Efya                       1
    Nonso Amadi                           1
    Olamide                               1
    Olu Maintain                          1
    Oxlade                                1
    P-Square                              2
    Phyno ft. Olamide                     1
    R2bees feat. Wande Coal               1
    Rema                                  1
    Runtown                               1
    Sarkodie feat. Castro                 1
    Skales                                1
    Styl-Plus                             1
    Tekno                                 1
    The Mavins                            1
    Timaya                                1
    Tiwa Savage & Don Jazzy               1
    Victony & Tempoe                      1
    Wande Coal                            1
    Wande Coal & DJ Tunez                 1
    Weird MC                              1
    Wizkid                                2
    Wizkid feat. Tems                     1
    Yemi Alade                            1
    Name: count, dtype: int64




```python
import matplotlib.pyplot as plt

df_afrobeats50["Artist"].value_counts().sort_index().plot(kind="bar", figsize=(10,6))
plt.title("Top Afrobeats Artist in the past Decade by number of Billboard Top 50 songs")
plt.xlabel("Artist")
plt.ylabel("Number of Songs")
plt.show()

```


    
![png](output_34_0.png)
    



```python
df_afrobeats50["Artist"].value_counts().head(10).plot(
    kind="barh", figsize=(8,6)
)

plt.title("Top Ten Afrobeats Artists by Number of Billboard Top-50 Songs")
plt.xlabel("Number of Songs")
plt.ylabel("Artist")
plt.show()

```


    
![png](output_35_0.png)
    


### Artist Concentration: Broad Influence Over Single Dominance

The bar chart of the top ten artists shows that most artists appear once, while only a select few appear twice. This clustering at low counts explains why the earlier plot appeared visually dense: Afrobeats’ influence is widely spread across many artists rather than concentrated in a single dominant figure. The few artists with multiple entries represent the pillars of the Afrobeats genre, whose work has consistently shaped its global narrative. However, it is important to note that a foundational artist like Tuface (2Baba), who has the decade's number one song, is not reflected as a pillar when considering his total number of songs on the chart, particularly when compared to counterparts like Davido, Wizkid, and Psquare. This low representation may be explained by 2Baba's switch after 2006 from his influential label (Kennis Music), which produced and promoted the number one song, to his personal record label.

### Conclusion
This analysis of Billboard’s 50 Best Afrobeats Songs provides insight into the temporal evolution and artistic concentration of Afrobeats over the past two decades. The year-based distribution highlights a clear intensification of Afrobeats’ global visibility in the mid-to-late 2010s, with notable peaks in 2016 and 2022. These periods align with Afrobeats’ transition from a regional genre to a globally recognized musical movement, driven by increased international collaborations, streaming platforms,scoail media and institutional recognition by Global music industries.
The artist-level analysis reveals that while Afrobeats is characterized by a diverse pool of contributors, a small group of artists, most notably Davido, Wizkid, Burna Boy, D’banj, and P-Square—appear multiple times within the Top 50 list. This pattern suggests the coexistence of both artist dominance and genre openness, where a few globally influential figures shape the genre’s international footprint while still allowing space for emerging and legacy acts to contribute defining works.
Overall, the findings reinforce Afrobeats as a dynamic, evolving genre shaped by both historical continuity and contemporary innovation. From early foundational tracks in the 2000s to the genre’s global peak in the 2020s, Afrobeats demonstrates sustained cultural relevance, creative diversity, and growing global influence.

### References:

Billboard Best Afrobeats Songs; https://www.billboard.com/lists/best-afrobeats-songs/

Trickle Media; https://tricklemedia.net/2face-idibia-a-lifetime-of-controversies/

Wikipedia Billboard U.S Afrobeats Songs; https://en.wikipedia.org/wiki/Billboard_U.S._Afrobeats_Songs


```python

```
