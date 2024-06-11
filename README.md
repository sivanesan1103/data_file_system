# data_file_system


## creat a vm (kali is better)
  * update repo
  * install python
  * intall pip
  * install instaloader
  * open firefox
  * add profile in firefox


### Install python
  ```
    sudo apt install python3.13
  ```

### Install pip
```
sudo apt install python3-pip
```

### Intall instraloader 
```
pip3 install instaloader
```

https://instaloader.github.io/

### creating firefox profile
```
firefox -p {name}
```

This the code is fix the maxinum try
```
from argparse import ArgumentParser
from glob import glob
from os.path import expanduser
from platform import system
from sqlite3 import OperationalError, connect

try:
    from instaloader import ConnectionException, Instaloader
except ModuleNotFoundError:
    raise SystemExit("Instaloader not found.\n  pip install [--user] instaloader")


def get_cookiefile():
    default_cookiefile = {
        "Windows": "~/AppData/Roaming/Mozilla/Firefox/Profiles/*/cookies.sqlite",
        "Darwin": "~/Library/Application Support/Firefox/Profiles/*/cookies.sqlite",
    }.get(system(), "~/.mozilla/firefox/*/cookies.sqlite") #location of cookie
    cookiefiles = glob(expanduser(default_cookiefile))
    if not cookiefiles:
        raise SystemExit("No Firefox cookies.sqlite file found. Use -c COOKIEFILE.")
    return cookiefiles[0]


def import_session(cookiefile, sessionfile):
    print("Using cookies from {}.".format(cookiefile))
    conn = connect(f"file:{cookiefile}?immutable=1", uri=True)
    try:
        cookie_data = conn.execute(
            "SELECT name, value FROM moz_cookies WHERE baseDomain='instagram.com'"
        )
    except OperationalError:
        cookie_data = conn.execute(
            "SELECT name, value FROM moz_cookies WHERE host LIKE '%instagram.com'"
        )
    instaloader = Instaloader(max_connection_attempts=1)
    instaloader.context._session.cookies.update(cookie_data)
    username = instaloader.test_login()
    if not username:
        raise SystemExit("Not logged in. Are you logged in successfully in Firefox?")
    print("Imported session cookie for {}.".format(username))
    instaloader.context.username = username
    instaloader.save_session_to_file(sessionfile)


if __name__ == "__main__":
    p = ArgumentParser()
    p.add_argument("-c", "--cookiefile")
    p.add_argument("-f", "--sessionfile")
    args = p.parse_args()
    try:
        import_session(args.cookiefile or get_cookiefile(), args.sessionfile)
    except (ConnectionException, OperationalError) as e:
        raise SystemExit("Cookie import failed: {}".format(e))
```
### location on cookie file in linux
```
~/.mozilla/firefox/
```
the 0m5pjlpw.default-esr is defult cookile whick contain the main profile are account cookie

by save above script and run in python script name it will download the defult main id and then 
create firefox profile and log the secound accout and then change the commeted pale * into the cooki name which is available in the loaction of cookies  

### To confirm the two session id is available nee to co th this dir:
```
cd ~/.config/instaloader 
```

### create a file named args1.txt limit 30
```
--login=login id
--highlights
--stories
--fast-update
abi__sridhar
bhavyasree__15
_.akshaya._.31
tifi_2003
``` 

### to run instaloader
```
instaloader +args1.txt +args2.txt
```
it shoude be same id if diifernt id differnt folder with differnt command 

https://ubuntu.com/tutorials/install-and-configure-samba#2-installing-samba

```
find . -not -name "*.json.xz" -mtime -1 -exec bash -c 'dir=$(basename "$(dirname "{}")"); mkdir -p /home/kali/backup/"$dir"; cp -r "{}" /home/kali/backup/"$dir"' \;

```
