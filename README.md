# Series Notify
This script checks for new series/anime episodes on supported websites. If it detects new episodes, a clickable GUI, implemented with Kivy, is launched.

## Download
- download a zip version [here](https://github.com/AttackRainbow/SeriesNotify/archive/master.zip) and extract all to an empty folder  
(or clone https://github.com/AttackRainbow/AnimeNotify.git)
- install [python 3.8.6](https://www.python.org/downloads/release/python-386/) or [above](https://www.python.org/downloads/) (or try with your existing python)
## Usage
1. simply run main.py
2. ???  
3. profit

run add.py to add more urls and titles  
run checkers.py to see all supported websites  
open urls.csv to see all saved urls and titles

NOTE:  
In case you want to check the same title in many websites, you can save the same title with different urls. The same title, whose line comes first (is added first), is the only one that gets reported.  
For example,  
```
# in urls.csv
url,title,ep
url_1,same title,1
url_2,same title,1
...,...,...
url_3,same title,1
```
If new ep is detected for "same title", only url_1 gets reported.

## Making a new checker for an unsupported website
Every checker in checkers.py is a fucntion that 
- needs to be named {web_signature}_checker
- takes 2 parameters, url="" and get_url_struct=False
- returns str(url struct of that website) if get_url_struct == True  
else returns int(all eps on website), str(link to latest ep)

To write your own checker function, copy the code below and paste into a new file named "lab.py" in the same directory as main.py and checkers.py.  
Redefine the _checker function to return values correctly based on url and get_url_struct.
```
# in lab.py

from utils import install
import constants
import requests


def _checker(url="", get_url_struct=False):
    if get_url_struct:
        # str(url struct of that website)
        return "http://anime-example.com/{id}/"

    # r = requests.get(url, headers=constants.headers)
    # todo: make it return those below

    num_ep = 10
    last_link = "http://anime-example.com/{id}/ep10"
    # return int(all eps on website), str(link to latest ep)
    return num_ep, last_link  # do not change


def test(url):
    try:
        print(_checker(url))
    except Exception as e:
        print(e)


if __name__ == "__main__":
    # print the result of your checker
    test(url="http://anime-example.com/543/")

    # fill these three below before installation
    key = "anime-example"  # identifier of the website
    # valid function name (ends with _checker)
    checker_name = "anime_example_checker"
    # url structure when copied from address bar
    # conventional struct elements: {id}, {title}, {ep}, {year}, {month}, ...
    url_structure = "http://anime-example.com/{id}/"

    # comment/uncomment below to install (will write to main.py and checkers.py directly)
    # install(key, checker_name, url_structure, _checker)

```
After finishing writing the body of your checker, you can test your checker by calling test(url="your_url"). After sucessful testing, fill in key, checker_name, and url_structure, then uncomment and call install() below.

The key must be the identifier of the website, for example
- for http://anime-example.com/{id}/, the key could be "anime-example" or "anime-example.com"
- for https://www.youtube.com/playlist?list=PLwLSw1_eDZl01_ftoIT3birJWkpxFZkEl, the key should only be youtube.com/playlist?list=, for youtube playlist only not for normal youtube videoes.
