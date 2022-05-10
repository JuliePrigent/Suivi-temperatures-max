#To launch on a google collab notebook

#settings to run on google collab

!pip install selenium
!apt-get update 
!apt install chromium-chromedriver
!cp /usr/lib/chromium-browser/chromedriver /usr/bin
import sys
sys.path.insert(0,'/usr/lib/chromium-browser/chromedriver')
from selenium import webdriver
chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument('--headless')
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--disable-dev-shm-usage')

#browser variable :
wd = webdriver.Chrome('chromedriver',chrome_options=chrome_options)

#Web scrap tools :

from selenium.webdriver.common.by import By
!pip install bs4
!pip install requests
import requests

from bs4 import BeautifulSoup as Soup
from urllib.request import urlopen as uReq

#Request the website :

wd.get('https://www.google.com/')
#cookie=wd.find_element(By.LINK_TEXT, "accepter et fermer")

barre_recherche= wd.find_element(By.NAME,"q")
barre_recherche.send_keys('Météo' + ' ' + input("Nom de la ville  : "))

from selenium.webdriver.common.keys import Keys
barre_recherche.send_keys(Keys.ENTER)

#Get the html version of the page 
source = wd.page_source
wd.quit()

page_soup = Soup(source, "html.parser")
containers = page_soup.findAll("div", {"class":"wob_df"})

#Collecting the required data :
tab = dict()
liste_jour= []
liste_max = []

for contain in containers:
  print(containers, contain)
  jour=contain.div["aria-label"]
  liste_jour.append(jour)

  div= contain.find_all('div',class_='gNCp2e')
  liste_maximale = [x.get_text() for x in div[0].find_all('span')]
  maximale = int(liste_maximale[1])
  liste_max.append(maximale)
  
tab["Jour"] = liste_jour
tab["Maximale"] = liste_max

print(tab)

#SET UO A LINE CHART

import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

df=pd.DataFrame(data=tab)
figure = df.plot()

# set the plot title.
plt.title('Températures maximales sur les 7 prochains jours')
#Removed the plot legend
figure.get_legend().remove()


#Define the y-axis scale 
plt.ylim(ymin=0,ymax=5+max(tab["Maximale"]))

#Add the y-axis label on the line 
for i, v in enumerate(tab["Maximale"]):
    figure.annotate(str(v), xy=(i,v), xytext=(-7,7), textcoords='offset points')

#Set the y-axis label (removed)
figure.axes.get_yaxis().set_visible(False)


#Set the x-axis label
tickvalues = df.index
plt.xticks(ticks = tickvalues ,labels = tab["Jour"], rotation = 'vertical')


#Add the city name on the plot
plt.figtext(0.5, 0.4,
            Nom_ville,
            horizontalalignment ="left", 
            verticalalignment ="bottom", 
            wrap = True, fontsize = 14)
plt.show()  
    
