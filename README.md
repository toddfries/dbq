# dbq
```
Subject: dbq - ultimate cooldown timer tracker for life, games, and more
From: Todd T. Fries <todd@fries.net>
To: anyone reading this
```

So you have lots of reasons to want to remember X in Y minutes.  Some games
have cooldown timers, but "Ok, I will check on X in 90 minutes."

This takes the basic concept of a cooldown timer and notifies you when its
done, with easy cli management and reset options.

I will do my best to describe how this works, but first, what is required:

```
use DBD::Pg qw(:pg_types);
use DBI qw(:sql_types);
use Date::Manip;
use FDC::db;
use Getopt::Std;
use POSIX qw(getpid);
use ReadConf;
use Term::ReadLine;
```

Note: `ReadConf` and `FDC::db` are also repositories of mine on github.

  - [ReadConf](https://github.com/toddfries/ReadConf)
  - [FDC::db](https://github.com/toddfries/FDC-db)

Note: While I didn't write anything crazy specific to Postgresql, and it
wouldn't take a ton of effort to add e.g. mysql or sqlite3, I'm developing
this against Postgresql.  Tested additions for any db welcome.

You'll need a dbq.conf file:

```
	mkdir -p $HOME/.config/dbq
	echo "dsn=dbi:Pg:dbname=dbq;host=mydbserver.example.com" > dbq.conf
	echo "user=john" >> dbq.conf
```

You'll need to create the dbq database:
```
	createdb -h mydbserver.example.com -O john dbq
```

If you don't want to have a password prompt every time, create the
$HOME/.pgpass file with proper syntax:
```
	cd
	touch .pgpass
	chmod 600 .pgpass
	echo mydbserver.example.com:*:*:john:supersecretpass >> .pgpass
```

Finally, you'll need to start it:

```
	$ dbq
	dbinfo=>{user} = 'john'
	dbinfo=>{dsn} = 'dbi:Pg:dbname=dbq;host=mydbserver.example.com'
	dbinfo=>{pass} = ''
	Connected to dsn 'dbi:Pg:dbname=dbq;host=mydbserver.example.com' .. dbms name 'PostgreSQL' ver '11.00.0700' .. oidname 'oid'
	dbq> ls

	Timer  Expired?  Duration    Stop Time           Description
	dbq> add 
	q name? spiffy reason
	q dur? 1hour 1min 1sec
	dbq> ls

	Timer  Expired?  Duration    Stop Time           Description
	    1. 0     1hour 1min 1sec 2022-06-03 17:58:05 spiffy reason
	dbq> add
	q name? curly moe show starts
	q dur? 10min
	dbq> ls /moe/

	Timer  Expired?  Duration    Stop Time           Description
	    2. 0              10mins 2022-06-03 17:07:26 curly moe show starts
	dbq> ls -t

	Timer  Expired?  Duration    Stop Time           Description
	    2. 0              10mins 2022-06-03 17:07:26 curly moe show starts
	    1. 0     1hour 1min 1sec 2022-06-03 17:58:05 spiffy reason
	dbq> ls left -t

	Timer  Expired?  Duration    Stop Time           Description
	    2. 0        9mins 42secs 2022-06-03 17:07:26 curly moe show starts
	    1. 0        1hour 21secs 2022-06-03 17:58:05 spiffy reason
	dbq> quit
	$
```

Commands at the `dbq>` prompt are as follows:

```
ls <filter>     :  show entries

on <filter>     :  turn on and reset timer for entries

off <filter>    :  turn off timer for an entry

add             :  add a new timer

rename <filter> :  rename an existing timer

del <filter>    :  delete a timer entry entirely
```

`<filter>` breaks down to one or more of the following:

```
	/regex/  : spaces only permitted if it is the only argument
	on       : show only timers that are active (default is all)
	off      : show only timers that are inactive (default is all)
	left	 : change duration to reflect time left instead of total time
	-t       : sort by stop time, oldest stop time at the top
```

Note 'regex' is case sensitive.

In practice, I leave one terminal open to do `data entry/update` and in another
I will run `dbq -p` which will:
  - use `xmessage` to send a pop-up notice (if $DISPLAY is set)
  - send a one liner to the terminal
  - call 'dwebhook <webhook> <from> <msg>' if {discord}->{webhook} is set in dbq.conf

If anyone actually uses this beyond me, reach out.  I do love feedback!

Enjoy!

Thanks,

```
--
Todd Fries .. todd@fries.net

 ____________________________________________
|                                            \  1.636.410.0632 (voice)
| Free Daemon Consulting, LLC                \  1.405.227.9094 (voice)
| http://FreeDaemonConsulting.com            \  1.866.792.3418 (voice)
| PO Box 16169, Oklahoma City, OK 73113-2169 \
| "..in support of free software solutions." \
 \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
                                                 
              37E7 D3EB 74D0 8D66 A68D  B866 0326 204E 3F42 004A
                        http://todd.fries.net/pgp.txt
```

If you find this useful and wish to donate, I accept donations:

- BTC: [168137ipuGTVDj55KE5v82xDD7QiGRBF44](bitcoin:168137ipuGTVDj55KE5v82xDD7QiGRBF44)

