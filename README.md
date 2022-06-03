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

