# ys-r

## Connect in R

		library(RODBC)
		sql <- odbcDriverConnect('DSN=[servername];UID=[username];PWD=[passwd])
		sqlQuery(sql, "select top(1) * from YapstoneDM..PaymentType")


## Setup Linux with drivers to connect to MSSQL server

Install the Unix packages and drivers to connect to MSSQL

> brew install unixodbc
> brew install freetds

Create a symlink from the `freetds` driver to something more usable in `/usr/local/lib`

> ln -s /usr/local/Cellar/freetds/0.95.18/lib/libtdsodbc.so /usr/local/lib/libtdsodbc.so

## Create the `.ini` files

> touch ~/.odbc.ini ; vim ~/.odbc.ini

`~/.odbc.ini`:
		[ODBC Data Sources]
		SERVERNAME     = servername

		[servername]
		Driver	    = /usr/local/lib/libtdsodbc.so
		Server      = [[servername]]
		User 	    	= [[redacted]]
		Password    = [[redacted]]
		Port        = 1433
		TDS_Version = 8.0

> touch ~/.odbc.ini ; vim ~/.odbc.ini

`~/.odbcinst.ini`:
		[MSSQL]
		Description   = Microsoft SQL Server driver
		Driver        = /usr/local/Cellar/freetds/0.95.18/lib/libtdsodbc.so

> touch ~/.freetds.conf ; vim ~/.freetds.conf

`~/.freetds.conf`:
		[global]
		;	tds version = 4.2
		;	dump file = /tmp/freetds.log
		;	debug flags = 0xffff
		;	timeout = 10
		;	connect timeout = 10
		text size = 64512

		[SERVERNAME]
			host = [[servername]]
			port = 1433
			tds version = 4.2

## In R, install `RODBC`
	
		install.packages("RODBC", type="source")

