# ESPN Team Calendar & Home Assitant

Please note that this uses the Google Calendars intergration and if you have not set that up yet, please do so. 
Instructions on how to do so can be found here https://www.home-assistant.io/integrations/google/

Although there is the ability to do with just google calendars, one of things I was not happy with was that it only showed team names. Since I follow college football & basketball I wanted to know the school my team (Michigan Wolverines) was playing against. I discovered by accident really that this was possible courtesy of ESPN and ROTK calendars. 

The thing I like about the calendar is that the data in the message gives a lot more information about the event. With this information I have been able to tell weather the game is a home or away, the name of the school, start time, and what channel the broadcast is on. 

Lets get started. 

To do this we go to ESPN.com, and on the sport tab click the sport you wish to follow. For this example I am using Collage Football (NCAAF). 

![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Screenshot%202021-12-16%20133728.png)
From this page you can go to the Teams tab which displays all the teams. Alternately, you can filter by conference as well. 

For this example I am using Big Ten and University of Michigan
![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Screenshot%202021-12-16%20133817.png)

Once on the page for your team, select schedule and they'll be a button to that says "Add to Calendar".

![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Screenshot%202021-12-16%20133857.png)

Follow the on-screen prompts as follows to add the calendar to google.

![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Screenshot%202021-12-16%20133941.png)

Here you can enter your TV information for game channel. 

![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Screenshot%202021-12-16%20134002.png)

Next, select the calendar you add the schedule too. In our case we will use Google Calendar. 
![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Screenshot%202021-12-16%20134042.png?raw=true)

Once selected, another pop up will appear asking permission for ROTK Calendar access to your google account. 

After authorization has completed, the calendar will now show up in your Google Calendar.
-----------------------------------------------------------------------------------------------
Once the calendar had been added to your Google account, a simple restart of Home Assistant and the calendar will appear in your calendar tab.

![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Picture3.png)

The calendar message itself will look like this: 

```
message: 🏈Georgia Bulldogs @ Michigan Wolverines
all_day: false
start_time: 2021-12-31 19:30:00
end_time: 2021-12-31 22:30:00
location: Watch on ESPN (Ch 206)
description: Watch the Georgia Bulldogs take on the Michigan Wolverines in the Cfp Semifinal At The Capital One Orange Bowl LIVE on WatchESPN! http://espn.go.com/watchespn/index?gameId=401331234&sport=college-football

Follow along online for live updates at: http://espn.go.com/college-football/game?gameId=401331234

Buy Tickets with Vivid Seats! https://www.vividseats.com/ncaaf/orange-bowl-tickets/orange-bowl-12-31-3596838.html?wsUser=717?gcid=CHAFF--GEOUS-_-CMPCALEXPORT-_-PARTESPN

Gear up at the ESPN Fan Shop powered by Ebay: https://calrep.ly/ncaafebay

Share - https://rokt.it/3jz35ot

You may manage your calendar subscription by following - https://espncfb.roktcalendar.com/preferences/8180b9df-bf39-4153-8ccd-ce219c238bd5

Powered by Rokt Calendar
offset_reached: false
friendly_name: NCAAF: Big Ten Conference - Michigan Wolverines
```
For basketball:

```
message: Southern Utah Thunderbirds @ Michigan Wolverines
all_day: false
start_time: 2021-12-18 19:00:00
end_time: 2021-12-18 22:00:00
location: Watch on BTN
description: Follow along online for live updates at: http://espn.go.com/ncb/gamecast?gameId=401372136

Buy Tickets with Vivid Seats! https://www.vividseats.com/ncaab/michigan-wolverines-tickets/wolverines-12-18-3704547.html?wsUser=717?gcid=CHAFF--GEOUS-_-CMPCALEXPORT-_-PARTESPN

Gear up at the ESPN Fan Shop powered by Ebay: https://calrep.ly/ncaafebay

Share - https://rokt.it/30BbZdu

You may manage your calendar subscription by following - https://espncbb.roktcalendar.com/preferences/b3bb6410-000f-4db1-8cf0-5ca7edad0525

Powered by Rokt Calendar
offset_reached: false
friendly_name: NCAAM: Big Ten Conference - Michigan Wolverines
````
From the information available, we can use that to create several sensors.

For example, I have a sensor that determines if its a home or away game. 

```
      - name: "MichiganBasketBallLocation"
        state: >
              {% set msg = state_attr("calendar.ncaam_big_ten_conference_michigan_wolverines", "message") %}
              {% set team1 = "Michigan Wolverines" %}
              {% if msg.index(team1) == 0 %}
                  Away
              {% else %}
                  Home
              {% endif %}
``` 


I have a  card set up  looking  like this.

![](https://github.com/afwelch/haespncalendar/blob/main/pictures/Picture1.png?raw=true) 

In the Template Sensors file, you will find the code used. 

