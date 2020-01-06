This is a fork of the nfldb module, which relies on the nflgame module. Both have been updated to make them work for the 2019 NFL season by making changes to handle the Chargers' move to Los Angeles and London games with an AM Eastern time scheduled start. 

### Installation Guide:

First, make sure that git and pip are installed on your system. 

Then run the following command as root (this will install the updated fork of nflgame):

pip install git+https://github.com/scripted-adventurer/nflgame.git

Next, install postgres and create a new database. These are the steps I followed on Ubuntu:
1. Run the following command as root:
apt-get install postgresql postgresql-contrib
2. Connect to psql using the default postgres user:
sudo -u postgres psql
3. Issue the following commands to create a new database, a new user, and give that user full permission to edit the database (where {password} is the password you wish to set):
CREATE DATABASE nfldb;
CREATE USER nfldb WITH ENCRYPTED PASSWORD '{password}';
GRANT ALL PRIVILEGES ON DATABASE nfldb TO nfldb;

Next, extract the database dump file (nfldb.sql) and import it to your database. Below is the command I used:

psql -U nfldb -h 127.0.0.1 nfldb < nfldb.sql

Then run the following command as root (this will install the updated fork of nfldb):

pip install git+https://github.com/scripted-adventurer/nfldb.git

Finally, you need to tell nfldb how to connect to the database. Find the sample configuration file (config.ini.sample) or download it from the repository. Open it with your favorite text editor and update the parameters to match the database you created (you should only need to update the password unless you used a different username or db name). Then copy it into your default configuration directory. Below are the commands I ran (from the directory where I saved the updated config.ini.sample):

mkdir -p $HOME/.config/nfldb

cp /etc/xdg/nfldb/config.ini.sample $HOME/.config/nfldb/config.ini



-original nfldb guide-

**THIS PROJECT IS UNMAINTAINED.**

nfldb is a relational database bundled with a Python module to quickly and
conveniently query and update the database with data from active games.
Data is imported from
[nflgame](https://github.com/BurntSushi/nflgame), which in turn gets its data
from a JSON feed on NFL.com's live GameCenter pages. This data includes, but is
not limited to, game schedules, scores, rosters and play-by-play data for every
preseason, regular season and postseason game dating back to 2009.

It can also
[be used with nflvid to search for and watch video of
plays](http://pdoc.burntsushi.net/nflvid.vlc). Please see the
[nfldb wiki](https://github.com/BurntSushi/nfldb/wiki) for more details on how
to get that working.

Here is a small teaser that shows how to use nfldb to find the top five passers
in the 2012 regular season:

```python
import nfldb

db = nfldb.connect()
q = nfldb.Query(db)

q.game(season_year=2012, season_type='Regular')
for pp in q.sort('passing_yds').limit(5).as_aggregate():
    print pp.player, pp.passing_yds
```

And the output is:

```bash
[andrew@Liger ~] python2 top-five.py
Drew Brees (NO, QB) 5177
Matthew Stafford (DET, QB) 4965
Tony Romo (DAL, QB) 4903
Tom Brady (NE, QB) 4799
Matt Ryan (ATL, QB) 4719
```


### Documentation and getting help

nfldb has
[comprehensive API documentation](http://pdoc.burntsushi.net/nfldb).
Tutorials, more examples and a description of the data model can be found
on the [nfldb wiki](https://github.com/BurntSushi/nfldb/wiki).

If you need any help or have found a bug, please
[open a new issue on nfldb's issue
tracker](https://github.com/BurntSushi/nfldb/issues/new)
or join us at our IRC channel `#nflgame` on FreeNode.


### Installation and dependencies

nfldb depends on the following Python packages available in
[PyPI](https://pypi.python.org/pypi):
[nflgame](https://pypi.python.org/pypi/nflgame),
[psycopg2](https://pypi.python.org/pypi/psycopg2),
[pytz](https://pypi.python.org/pypi/pytz) and
[enum34](https://pypi.python.org/pypi/enum34).
nfldb also needs PostgreSQL installed with an available empty database.

I've only tested nfldb with Python 2.7 on a Linux system. In theory, nfldb
should be able to work on Windows and Mac systems as long as you can get
PostgreSQL running. It is not Python 3 compatible (yet, mostly because of
the `nflgame` dependency).

Please see the
[installation guide](https://github.com/BurntSushi/nfldb/wiki/Installation)
on the [nfldb wiki](https://github.com/BurntSushi/nfldb/wiki)
for instructions on how to setup nfldb.


### Entity-relationship diagram

Here's a condensed version that excludes play and player statistics:

[![Shortened ER diagram for nfldb](http://burntsushi.net/stuff/nfldb/nfldb-condensed.png)](http://burntsushi.net/stuff/nfldb/nfldb-condensed.pdf)

There is also a [full PDF version](http://burntsushi.net/stuff/nfldb/nfldb.pdf)
that includes every column in the database.

The nfldb wiki has a [description of the data
model](https://github.com/BurntSushi/nfldb/wiki/The-data-model).

The most recent version of the nfldb PostgreSQL database is available here:
[http://burntsushi.net/stuff/nfldb/nfldb.sql.zip](http://burntsushi.net/stuff/nfldb/nfldb.sql.zip).

