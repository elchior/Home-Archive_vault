---
tags: [state/closed/2022/11]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:31:14.650+02:00
---

# T2022-1017-0956 Python Project - MapIt
#state/closed/2022/11
## Description
Basic Function:
Get a post address to open google maps at this location.

## Documentation
Setup
```bash
conda deactivate
cd ~/Documents/python/mapit/
source bin/activate
```
- Create project ~/Documents/python/mapit
  [[Python#Create a new venv for a project]]
- Create bash wrapper script for venv (mapit.bsh)
- Install python packages
  ```bash
  pip install wheel
  pip install pyperclip
  ```
- Required OS packages
  - xsel (for clipboard)

Get coordinates for address
- Install geopy
  ```bash
  pip install geopy
  ```
- Use geopy to get the world coordinates for an address
  ```python
  >>> geolocator = Nominatim(user_agent="mapit")
  >>> location = geolocator.geocode("Burgdorf Eyzaelg 24")
  >>> print(location.address)
  24, Eyzälg, Burgdorf, Verwaltungskreis Emmental, Verwaltungsregion Emmental-Oberaargau, Bern/Berne, 3400, Schweiz/Suisse/Svizzera/Svizra
  >>> print(location.latitude,location.longitude)
  47.06952865 7.620932649999999
  >>> print(location.raw)
  {'place_id': 161006372, 'licence': 'Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright', 'osm_type': 'way', 'osm_id': 225177213, 'boundingbox': ['47.069455', '47.0696023', '7.6208261', '7.6210392'], 'lat': '47.06952865', 'lon': '7.620932649999999', 'display_name': '24, Eyzälg, Burgdorf, Verwaltungskreis Emmental, Verwaltungsregion Emmental-Oberaargau, Bern/Berne, 3400, Schweiz/Suisse/Svizzera/Svizra', 'class': 'building', 'type': 'house', 'importance': 0.22010000000000002}
  ```
- Install pyproj
  ```bash
  pip install pyproj
  ```
- Convert coorinates
  ```python
  >>> transformer = Transformer.from_crs("epsg:4326","epsg:2056")
  >>> transformer.transform(location.latitude,location.longitude)
  (2613847.313511985, 1213183.794799811)
  ```
- Use selenium
  - Instalation
    ```bash
    pip install selenium
    ```

Current Code
```python
# mapit - script to open the google map at a given address.
# the address will be taken from the clipboard if not given on the command line.
#
# Created: 2022-10-17
# Version: 0.1
# Author: ME

import webbrowser, sys, pyperclip, re
from geopy.geocoders import Nominatim
from pyproj import Transformer

urllist=[
'https://www.google.ch/maps/place/',
'https://map.search.ch/',
'https://map.schweizmobil.ch/?lang=de&photos=yes&bgLayer=pk&season=summer&resolution=1&detours=yes&logo=yes'
]

def get_search():
    # If an argument was given on the command line
    if len(sys.argv) > 1:
        # Get all commandline args except the program name itselfe
        search=' '.join(sys.argv[1:])
    else:
        # Else get the address from the clipboard
        search=pyperclip.paste()
    return search

def search_to_coordinates(search: str) -> tuple:
    # Get world coordinates for the location to find
    geolocator = Nominatim(user_agent="mapit")
    location=geolocator.geocode(search)
    # Return a tuple with latitude,longitude
    return location[1]

def coordinates_world_to_swiss(coordinates_tuple) -> tuple:
    # Transform global coordinates to swiss coordinates
    transformer = Transformer.from_crs("epsg:4326","epsg:2056")
    swisscoordinates_N,swisscoordinates_E=transformer.transform(coordinates_tuple[0],coordinates_tuple[1])
    return round(swisscoordinates_N),round(swisscoordinates_E)

def create_schweizmobil_url(coordinates_tuple):
    coordinates_url="&E={}&N={}".format(coordinates_tuple[0],coordinates_tuple[1])
    return coordinates_url

def url_append(base_url: str,search: str):
    if re.search("schweizmobil",base_url):
        world_coordinates=search_to_coordinates(search)
        swiss_coordinates=coordinates_world_to_swiss(world_coordinates)
        coordinates_appendix=create_schweizmobil_url(swiss_coordinates)
        complete_url=base_url+coordinates_appendix
    else:
        complete_url=base_url+search
    return complete_url

def openmap(url: str):
    if re.search("schweizmobil.ch",url):
        webbrowser.open(url)
    else:
        webbrowser.open(url)

def main():
    search=get_search()
    for url in urllist:
        goto=url_append(url,search)
        openmap(goto)

if __name__ == '__main__':
    main()
```
## Issues
Python package issue with geopy. The tools seems to work anyway
```bash
python -m pip check
geopy 2.2.0 has requirement geographiclib<2,>=1.49, but you have geographiclib 2.0.
```
## Todo
- Make it easier to use the tool from the desktop.
  - keyboard short cut, small gui with search bar?
- Verify if the search on swissmobil has returned a place in switzerland. Example search for "Sion" returns coordinates for a city in france
## Done
### 2022-10-17
- Created Task
- Setup project and install tools/libraries
- request to SchweizMobil if it is possible to use their url with a search pattern to find a place on their map.
- It looks like they have the swiss coordinates in the url after a search on the website.
- An option might be to get the global coordinates from a website. Could be google or an other. And then convert the global coordinates with Lat / Long to the swiss system.
- There is a python library that can do the conversion from global to swiss coordinates:
   <https://ia.arch.ethz.ch/lat-lon-to-ch-coordinates/>

### 2022-10-18
- The URL looks like this with the swiss coordinate system given for an address:
  <https://map.schweizmobil.ch/?lang=de&photos=yes&bgLayer=pk&season=summer&resolution=1&E=2613767&N=1213208&detours=yes&logo=yes>
   - The above url displays the map on the spot around the coordinates which are
     - E=2613767
     - N=1213208
   - It does not show the exact location (mark on the map)
- This website from openstreetmap can give the global coordinates for an address
    <https://nominatim.openstreetmap.org/ui/search.html?q=eyzaelg+24+burgdorf>

- How to get latitude and longitude from the open streetmap project.
  - Install geopy
    ```bash
    pip install geopy
    ```
  - Find out how to get coordinates for an address / location search
  - Find out how to convert global coordinates to swiss coordinates

### 2022-10-19
- Information about swiss coordiante system
   <https://www.swisstopo.admin.ch/de/wissen-fakten/geodaesie-vermessung/koordinaten/schweizer-koordinaten.html>
   <https://de.wikipedia.org/wiki/Schweizer_Landeskoordinaten>
- Create URL for SchweizMobil with the searched and converted coordinates.
- First working draft version opening three maps in brave.
- Created more structured code with lots of functions
### 2022-10-21
- Create a script for the environment setup and a requirements.txt file (freeze option?)
  [[Python#requrements]]
- Upgrade python modules
  [[Python#Change venv]]
- Added commandline arguments for debugging
  [[Python#Debuging]]
- [[Visual Studio Code#Howto#Github / Feature Branch]]
- Use selenium to open a new specific browser with the three tabs.
  - Download chrome driver
    ```bash
    cd ~/Documents/python/mapit
    wget https://chromedriver.storage.googleapis.com/106.0.5249.61/chromedriver_linux64.zip
    mv chromedriver bin/
    rm chromedriver_linux64.zip
    ```

Current code (backup before git usage...)
```bash
# mapit - script to open the google map at a given address.
# the address will be taken from the clipboard if not given on the command line.
#
# Created: 2022-10-17
# Version: 0.1
# Author: ME

from ssl import Options
import webbrowser, sys, pyperclip, re
from geopy.geocoders import Nominatim
from pyproj import Transformer
from selenium import webdriver

urllist=[
'https://www.google.ch/maps/place/',
'https://map.search.ch/',
'https://map.schweizmobil.ch/?lang=de&photos=yes&bgLayer=pk&season=summer&resolution=1&detours=yes&logo=yes'
]

def get_search():
    # If an argument was given on the command line
    if len(sys.argv) > 1:
        # Get all commandline args except the program name itselfe
        search=' '.join(sys.argv[1:])
    else:
        # Else get the address from the clipboard
        search=pyperclip.paste()
    return search

def search_to_coordinates(search: str) -> tuple:
    # Get world coordinates for the location to find
    geolocator = Nominatim(user_agent="mapit")
    location=geolocator.geocode(search)
    # Return a tuple with latitude,longitude
    return location[1]

def coordinates_world_to_swiss(coordinates_tuple) -> tuple:
    # Transform global coordinates to swiss coordinates
    transformer = Transformer.from_crs("epsg:4326","epsg:2056")
    swisscoordinates_N,swisscoordinates_E=transformer.transform(coordinates_tuple[0],coordinates_tuple[1])
    return round(swisscoordinates_N),round(swisscoordinates_E)

def create_schweizmobil_url(coordinates_tuple):
    coordinates_url="&E={}&N={}".format(coordinates_tuple[0],coordinates_tuple[1])
    return coordinates_url

def url_append(base_url: str,search: str):
    if re.search("schweizmobil",base_url):
        world_coordinates=search_to_coordinates(search)
        swiss_coordinates=coordinates_world_to_swiss(world_coordinates)
        coordinates_appendix=create_schweizmobil_url(swiss_coordinates)
        complete_url=base_url+coordinates_appendix
    else:
        complete_url=base_url+search
    return complete_url

def openmaps(url: list):
        browser_path="/usr/bin/brave-browser"
        driver_path="bin/chromedriver"
        options=webdriver.ChromeOptions()
        options.binary_location=browser_path
        browser=webdriver.Chrome(options=options,executable_path=driver_path)
        for number,url in enumerate(urllist):
            if number == 0:
                browser.get(url)
            else:
                js_string="window.open('about:blank','tab_"+str(number+1)+"');"
                print(js_string)
                browser.execute_script(js_string)
                browser.switch_to.window("tab_"+str(number+1))
                browser.get(url)

def main():
    search=get_search()
    goto=[]
    for url in urllist:
        goto.append(url_append(url,search))
    openmaps(goto)

if __name__ == '__main__':
    main()
```

### 2022-10-27
small script to set mapit environment
```bash
#!/bin/bash
conda deactivate
cd ~/Documents/python/mapit
source bin/activate
```

Alias to source the above script
```bash
alias mapit_env='source $(which mapit_env.bsh)'
```

Now jump into the project with
```bash
mapit_env
```

### 2022-11-30
Documentation
[[Mapit]]
