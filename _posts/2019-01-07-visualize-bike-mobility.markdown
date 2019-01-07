---
title: "️🗺️ Visualizing bike mobility in London using interactive maps and animations"
layout: post
date: 2019-01-07 23:00
published: true
headerImage: false
tag:
- visualization
- bike
- Python
- DataScience
category: blog
author: edenau
description: "visualizing bike sharing systems"
# jemoji: '<img class="emoji" title=":open_file_folder:" alt=":open_file_folder:" src="https://assets.github.com/images/icons/emoji/unicode/1f5c2.png" height="20" width="20" align="absmiddle">'
---

# Introduction

Bike sharing systems have become popular means of travel in recent years, providing a green and flexible transportation scheme to citizens in metropolitan areas. Many governments in the world have seen this as an innovative strategy that could potentially bring a number of societal benefits. For instance, it could reduce the use of automobiles and hence reduce greenhouse gas emission and alleviate traffic congestion in city centres.

> <a href="http://content.tfl.gov.uk/attitudes-to-cycling-2016.pdf" target="_blank">Reports</a> have shown that 77% of Londoners agree that cycling is the fastest way to make short-distance journeys. In the long run, it might also help increase the life expectancy in the city.

I have been working on a data-driven cost-effective algorithm for optimizing (re-balancing) the system of Santander Cycles, a public bicycle hire scheme in London, which I should make a post about it once my paper is published (more info here). Before really working on the algorithm, I had to delve into loads of data and it would really help if I could somehow ***visualize*** them.

>> TL;DR - Let's see how we can visualize a bike sharing system using graphs, maps, and animations.

*You can find web maps in this <a href="https://edenau.github.io/maps/" target="_blank">page</a>. Most maps, animations, and source codes are available on <a href="https://github.com/edenau/maps" target="_blank">GitHub</a>. My work is inspired by <a href="https://blog.prototypr.io/interactive-maps-with-python-part-1-aa1563dbe5a9" target="_blank">Vincent Lonij</a> and <a href="https://towardsdatascience.com/master-python-through-building-real-world-applications-part-2-60d707955aa3" target="_blank">Dhrumil Patel</a>. Check out their posts!*

## Table of Contents
- [More about Data - the Boring Bit](#data)
- [Bar charts](#bar)
- [Interactive Maps](#interactive)
- [Density Maps](#density)
- [Connection Maps](#connection)
- [Animations](#animations)
- [Conclusions](#conclusions)
- [Remarks](#remarks)

<div class="breaker"></div> <a id="data"></a>

# More about Data - the Boring Bit

I obtained data of bicycle journeys from Transport for London (TfL). Every single bike journey made in their system since 2012 was recorded and those open data are available <a href="https://cycling.data.tfl.gov.uk/" target="_blank">online</a>.

A 36-day record of journeys made from 1 August to 13 September 2017 were analyzed. During this period, there were >1.5 million journeys made among >700 bike docking stations in London. From 2014, we have witnessed a >190% rise in bike trips made. The number of bicycles and docking stations in the system both increased more than twofold to accommodate the significant rise in cycling demand in central London and regional areas. Exact data will be shown in my soon-to-be-released paper. Stay tuned.

## Data Manipulation
I believed there would be a massive difference in journey patterns between weekdays and weekends. Let's do some coding and see if this is true. We first import data of journeys by `pd.read_csv()`.

```
# Load journey data
f = 'journeys.csv'
j = pd.read_csv(f)
date = j['date'].values
month = j['month'].values
year = j['year'].values
hour = j['hour'].values
minute = j['minute'].values
station_start = j['id_start'].values
station_end = j['id_end'].values
```

Then we extract weekday data by `date.weekday()` and evenly divide a 24-hour day into 72 time slices, such that each time slice represents a 20-minute interval.

```
# Compute IsWeekday
weekday = np.zeros(len(date))
weekday[:] = np.nan
cnt = 0
for _year, _month, _date, _hour, _minute in zip(year, month, date, hour, minute):
  _dt = datetime.datetime(_year, _month, _date, _hour, _minute)
  _weekday = _dt.weekday()
  weekday[cnt] = _weekday
  cnt += 1
IsWeekday = weekday < 5
j['IsWeekday'] = IsWeekday
# Compute TimeSlice
j['TimeSlice'] = (hour*3 + np.floor(minute/20)).astype(int)
```

We also need to check if those bike trips were made from/to abolished stations, as there are no ways to obtain information of those stations, such as locations, station name etc. (good job TfL). We labelled them as 'invalid' journeys.

```
# Load station data
f = 'stations.csv'
stations = pd.read_csv(f)
station_id = stations['station_id'].values
# Extract valid journeys
valid = np.zeros(len(date))
valid[:] = False
cnt = 0
for _start, _end in zip(station_start, station_end):
  if np.logical_and((_start in station_id), (_end in station_id)):
    valid[cnt] = True
  cnt += 1
j['Valid'] = valid
```

Finally, we only keep journeys that are 'valid' **and** made on weekdays, which turns out to be around 73% of the data.

```
df = j[j["IsWeekday"] == True].drop(columns="IsWeekday")
df = df[df["Valid"] == True].drop(columns="Valid")
print('Ratio of valid journeys= {:.2f}%'.format(df.shape[0] / j.shape[0] * 100))
```

<div class="breaker"></div> <a id="bar"></a>

# Bar Charts

We finally delve into the visualization part! The simplest forms of data visualization are arguably charts. By a simple `groupby('TimeSlice')` function, we can see how frequent journeys were made in different hours.

```
grp_by_timeslice = df.groupby('TimeSlice').count().values[:,0]
plt.bar(range(0,72), grp_by_timeslice)
plt.xlabel('Time Slice')
plt.ylabel('Departures')
plt.show()
```

![Average departure rates on weekdays (left) and weekends (right)]({{ site.url }}/assets/posts/visualize-1.png)

See? our hypothesis is right! Journey patterns on weekdays and weekends are so different, as we can see two peak hours on weekdays, where most people commute, but not on weekends. We can also observe the distribution of journey duration and speed in a similar fashion.

![Distribution of journey duration (left) and speed (right)]({{ site.url }}/assets/posts/visualize-2.png)

Note that we assumed straight paths were taken due to data constraint (they don't track your movement), which would be shorter than the actual paths, and therefore the speeds computed with distances between start and end stations would be ***underestimated***. If customers return the bike at the same station it was rent, the computed speed would be 0, which explains the rather strange spike spotted at 0 km/h.

<div class="breaker"></div> <a id="interactive"></a>

# Interactive Maps

If charts are fancy, maps are fancier. We will use `<a href="https://python-visualization.github.io/folium/" target="_blank">folium</a>`, which is a Python wrapper of <a href="https://leafletjs.com" target="_blank">Leaflet.js</a> that makes interactive maps. Make sure you install the latest version by

```
$ pip install folium==0.7.0
```

(or its `conda install` equivalent). I worked on Google Colaboratory and the pre-installed version is `0.2.0` with minimal functionalities.

I built a simple template for generating a map with circle markers (with different colours!) using clusters.

```
import folium
# Change colours
def color_change(c):
    if(c < 15):
        return('red')
    elif(15 <= c < 30):
        return('orange')
    else:
        return('green')
# Create base map
London = [51.506949, -0.122876]
map = folium.Map(location = London,
                 zoom_start = 12,
                 tiles = "CartoDB positron")
marker_cluster = MarkerCluster(locations=[lat, lon]).add_to(map)
# Plot markers
for _lat, _lon, _cap, _name in zip(lat, lon, cap, name):
    folium.CircleMarker(location = [_lat, _lon],
                        radius = 9,
                        popup = "("+str(_cap)+") "+_name,
                        fill_color = color_change(_cap),
                        color = "gray",
                        fill_opacity = 0.9).add_to(marker_cluster)

f = 'map_station_cluster.html'
map.save(f)
```

Why clusters? `MarkerCluster()` enables markers to cluster together when they are too close when you zoom out. You do not want your maps to be too messy, with markers overlapping one another.

![Station cluster map]({{ site.url }}/assets/posts/station-cluster-1.png)

It automatically de-clusters/unfolds when you zoom in:

![Station cluster map - zoomed in]({{ site.url }}/assets/posts/station-cluster-2.png)

But I promised you ***interactive*** maps. You can set `popup` parameter and display station name and its capacity when you click on it. Bravo!

![Interactions in station cluster map]({{ site.url }}/assets/posts/station-cluster-3.png)

*This map is available on <a href="https://edenau.github.io/maps/station-cluster/" target="_blank">https://edenau.github.io/maps/station-cluster/</a>.*

<div class="breaker"></div> <a id="density"></a>

# Density Maps

The above code used dynamic colour scheme that depends on the capacity of stations. We can also implement a dynamic radius scheme for those circle markers that is based on the number of departures and arrivals of each station. We can obtain what we called ***density maps*** that shows the net departures/arrivals of every station.

```
def DensityMap(stations, cnt_departure, cnt_arrival):
London = [51.506949, -0.122876]

  map = folium.Map(location = London,
                   zoom_start = 12,
                   tiles = "CartoDB dark_matter")

  stations['Total Departure'] = cnt_departure
  stations['Total Arrival'] = cnt_arrival
for index, row in stations.iterrows():
    net_departure = row['Total Departure'] - row['Total Arrival']

    _radius = np.abs(net_departure)
    if np.isnan(_radius):
      _radius = 0

    if net_departure > 0:
      _color= '#E80018' # target red
    else:
      _color= '#81D8D0' # tiffany blue

    lat, lon = row['lat'], row['lon']
    _popup = '('+str(row['capacity'])+'/'+str(int(_radius))+') '+row['station_name']

    folium.CircleMarker(location = [lat,lon],
                        radius = _radius,
                        popup = _popup,
                        color = _color,
                        fill_opacity = 0.5).add_to(map)

  return map
```

I used a different colour scheme here (*target red* and *tiffany blue* are both trademarked colours in the United States) that shows if the number of departures at some station is more or less than the number of arrivals. Big circle markers represent large departure-arrival difference.

Let's see how density maps look like in morning and evening peak hours:

```
# Select peak hours
TimeSlice = [25,53] # morning and evening
keyword = ['map_morning', 'map_evening']
# Journeys depart between 0820 and 0859, and between 1740 and 1819
for ts, kw in zip(TimeSlice, keyword):
  df_1 = df[df["TimeSlice"] == ts]
  df_2 = df[df["TimeSlice"] == (ts+1)]
  df_target = df_1.append(df_2)
cnt_departure = df_target.groupby("id_start").count().iloc[:,0]
  cnt_arrival = df_target.groupby("id_end").count().iloc[:,0]
vars()[kw] = DensityMap(stations, cnt_departure, cnt_arrival)
```

![Density map in morning peak hours]({{ site.url }}/assets/posts/density-1.png)

![Density map in evening peak hours]({{ site.url }}/assets/posts/density-2.png)

*Density maps are available on <a href="https://edenau.github.io/maps/density-morning/" target="_blank">https://edenau.github.io/maps/density-morning/</a> and <a href="https://edenau.github.io/maps/density-evening/" target="_blank">https://edenau.github.io/maps/density-evening/</a>.*

<div class="breaker"></div> <a id="connection"></a>

# Connection Maps

All the aforementioned maps focus on stations but not journeys, but we can also visualize journeys by what we called ***connection maps***, by simply drawing trips completed on the map. Without delving into much details:

![Connection map]({{ site.url }}/assets/posts/connection-1.png)

We can also add multiple layers of connection by `folium.LayerControl()` to separate layers of frequently and infrequently used paths.

![Multi-layer connection map]({{ site.url }}/assets/posts/connection-2.png)

*Connection maps are available on <a href="https://edenau.github.io/maps/connection-morning/" target="_blank">https://edenau.github.io/maps/connection-morning/</a> and <a href="https://edenau.github.io/maps/connection-morning-layers/" target="_blank">https://edenau.github.io/maps/connection-morning-layers/</a>.*

<div class="breaker"></div> <a id="animations"></a>

# Animations

So far we have demonstrated how to visualize temporal and distributional information by charts, and spatial information by various kinds of maps. But what if we generate multiple maps on consecutive time instances? We can visualize *spatio-temporal* information using animations!

The maps generated are web maps in *.html* files. The idea is to:

> *Generate a map for every time instance, browse it on a web browser, take a screenshot and save the picture, and link all pictures together as a video or a .gif file.*

We are going to automate the web browsing and screen capturing process by `<a href="https://www.seleniumhq.org/" target="_blank">selenium</a>`. We also need a web driver, and as a Chrome user I went for `chromedriver`.

```
from selenium import webdriver
def a_frame(i, frame_time, data):
    my_frame = get_image_map(frame_time, data)

    # Save the web map    
    delay = 5 # give it some loading time
    fn = 'frame_{:0>5}'.format(i)
    DIR = 'frames'
    f = DIR + '/' + fn + '.html'
    tmpurl='file://{path}/{mapfile}'.format(path=os.getcwd()+/frames',mapfile=fn)
    my_frame.save(f)
    # Open the web map and take screenshot
    browser = webdriver.Chrome()
    browser.get(tmpurl)
    time.sleep(delay)
    f = DIR + '/' + fn + '.png'
    browser.save_screenshot(f)
    browser.quit()
    f = 'frames/frame_{:0>5}.png'.format(i)
    image = Image.open(io.BytesIO(f))
    draw = ImageDraw.ImageDraw(image)
    font = ImageFont.truetype('Roboto-Light.ttf', 30)

    # Add text on picture
    draw.text((20, image.height - 50),
              'Time: {}'.format(frame_time),
              fill=(255, 255, 255),
              font=font)

    # Write the .png file
    dir_name = "frames"
    if not os.path.exists(dir_name):
        os.mkdir(dir_name)
    image.save(os.path.join(dir_name, 'frame_{:0>5}.png'.format(i)), 'PNG')
    return image
```

We can then make the video or gif by `<a href="https://www.ffmpeg.org/" target="_blank">ffmpeg</a>`. For Mac user, installing `ffmpeg` can literally take minimal effort with the aid of <a href="https://brew.sh/" target="_blank">Homebrew</a>, as

>> Homebrew installs the stuff you need that Apple didn’t.

After installing Homebrew (with a single command), simply type

```
$ brew install ffmpeg
```

and voilà! To create a *.mp4* file, try

```
$ ffmpeg -r 10 -i frames/frame_%05d.png -c:v libx264 -vf fps=25 -crf 17 -pix_fmt yuv420p video.mp4
```

For *.gif* file, try

```
$ ffmpeg -y  -t 3 -i frames/frame_%05d.png \ -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png
$ ffmpeg -r 10  -i frames/frame_%05d.png -i palette.png -filter_complex \ "fps=10,scale=720:-1:flags=lanczos[x];[x][1:v]paletteuse" animation.gif
```

Check out the animation of density maps throughout a day:



<div class="breaker"></div> <a id="conclusions"></a>

<div class="breaker"></div> <a id="remarks"></a>


<a href="https://edenau.github.io/maps/static" target="_blank">Static Map</a>

https://edenau.github.io/maps/morning

https://edenau.github.io/maps/evening
