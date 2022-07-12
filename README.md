# IRC weather bot for python3

### Installation:

1. Install python 3: `sudo apt install python3`
1. Install virtualenv: `sudo apt install python-virtualenv`
2. Ensure libffi and nano are installed: `sudo apt install build-essential libssl-dev libffi-dev nano`
3. Create/go to virtual environment directory: `mkdir -p ~/virtualenvironment/dziwx3 | cd ~/virtualenvironment`
4. Clone repository: `git clone https://github.com/dziban303/dziwx3.git ~/virtualenvironment/dziwx3`
5. Set up and activate virtualenv: 
   ```bash
   virtualenv -p python3 ~/virtualenvironment/dziwx3
   source ~/virtualenvironment/dziwx3/bin/activate
   cd ~/virtualenvironment/dziwx3/pywx
   ```
6. Install requirements.txt: `pip install -r requirements.txt`
7. Edit config file with nano `nano ~/virtualenvironment/dziwx3/pywx/example_config.py`:
   - forecast_io_secret = API key for forecast.io / darksky.com
   - youtube_key = API key for Google services (used for geocoding and elevation finding, as well as Youtube.)
   - host, port = IRC server and port
   - nick = bot's nickname
   - pass = bot's IRC password
   - chans = Channels to join. Must use preceeding #, e.g. `"chans": ["#weather,#dziwx"],`
   - twitch, imgur, redlink fields = API keys for optional features
   - leave other stuff alone
   - save and rename to `local_config.py`; in nano, save the file by typing `Ctrl+O` and then give it the new filename `local_config.py` when prompted.
8. Test the bot: `python3 pywx.py local_config.py`
9. To quit: `Ctrl-C` terminates the bot and returns you to the shell.
1. Create a shell script to run the bot: `nano ~/dziwx.sh`
   ```bash
   #!/bin/bash
   source ~/virtualenvironment/dziwx3/bin/activate
   cd ~/virtualenvironment/dziwx3/pywx
   python3 pywx.py local_config.py &
   ```
1. Make script executable: `sudo chmod 777 ~/dziwx.sh | sudo cp dziwx.sh /usr/local/bin/dziwx.sh`
   - The bot can now be launched by typing `dziwx.sh`

### Commands:
  - wx [location] — current weather conditions for [location]
  - wf [loc] — weather forecast
  - hf (-d|-w|-p) [loc] — hourly forecast
      - -d — dewpoint forecast
      - -w — wind forecast
      - -p — precipitation forecast
      - -t — temperature forecast      
      - -z — temperature and dewpoint forecast
      - -u — relative humidity forecast
      - -a — apparent temperature forecast
  - hfx [loc] — hourly forecast in sparklines
  - alerts [loc] — show weather alerts, if any, for a location
  - alert [alert number] — get detailed info about an alert shown in `alerts` or `wx`
  - wxtime|sun|moon [loc] — shows the time, sunrise/sunset times, length of day, and moon phase
  - find|locate|latlong [loc] — find coordinates and elevation of a location
  - eclipse [loc] — information about the next eclipse (currently, 14 October 2023) for a location
  - nextnexteclipse [loc] — information about the next next eclipse (currently, 8 April 2024) for a location
  - lasteclipse [loc] — information about the August 2017 total eclipse
  - swx — current space weather
  - swf — space weather forecast
  - lastquake — information about the last large earthquake detected
  - define [acronym] — find definitions of weather-related acronyms

#### Notes: 
- To add/edit acronyms for the `define` command, edit `acro.json`
 - To add/edit airports, edit `airports.dat`
 - To join password-protected channels, enter the password after the channel name in the `"chans"` field
 - To join multiple servers:
   - Copy `local_config.py` to `new_config.py` or whatever you want to call it, and populate it with the new server info
   - Copy `pywx.py`, name it `pywx2.py` or whatever you want to call it, and change the line therein from `from local_config import config` to `from new_config import config` replacing `new_config` with whatever your config file is named
   - Run `python3 pywx2.py`
   - Simultaneous use: `python3 pywx.py & python3 pywx2.py && fg`

#### Airports.dat formatting:
 The `airports.dat` file is from the OpenFlights database. More info can be found [here](https://openflights.org/data.html).
 - Fields: `airport_id,name,city,country,faa,icao,lat,long,alt,tz,dst`
   - **Airport_id** is a unique identifier. When adding new airports, just use a sequential number.
   - **Name** is the common name of the airport.  
   - **City** and **country** are difficult concepts to explain so I will just hope you know what they are.  
   - The **FAA** code is a three-to-five alphanumeric code unique to the airport; in many cases it is identical to the IATA code.  
   - The **ICAO** code is yet another unique identifier, and consists of a four-letter code. (For more on these codes, see [this wikipedia page](https://en.wikipedia.org/wiki/Location_identifier) 
   - **Latitude** and **longitude** are the airport's coördinates.  
   - **Altitude** is the airport's height above sea level, *in feet*. 
   - **Tz** is the *standard time* timezone offset from UTC; e.g., Eastern Standard Time is UTC-5, so `-5` is entered, while Japan Standard Time is UTC+9, so `+9` is used.
   - **Dst** indicates when Daylight Saving Time (or Summer Time) is used: Use `E` (Europe), `A` (US/Canada), `S` (South America), `O` (Australia), `Z` (New Zealand), `N` (None) or `U` (Unknown).
