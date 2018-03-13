---
title: Get PIA apps from scratch on OSx part 1 (Back End)
layout: post
permalink: 2018/03/03/Get-PIA-app-from-srcatch-p1/
tags: [PIA, Ruby, Rails, Postgres]
---
Set up the Back End app on OSx

# Introduction

Of course you play with the PIA Tool on your desktop as standalone or native application, to do so, please refere to the latest built from the [CNIL website](https://www.cnil.fr/fr/outil-pia-telechargez-et-installez-le-logiciel-de-la-cnil). For any other people interested to provide this tool in their company (or their consutling team) to enjoy the collaborative workflow, keep reading. This a serie of two posts to find out the main steps to get a full working version of the Back End and the Front End of PIA applications.

# Prepare your environment

## Assumptions 

1. Your computer is connected to the internet
2. You're testing the application in a development environment - NO PRODUCTION USE

## Environment settings 

The following configuration is used to review this application.

1. OS : OSx 10.13.3
2. Package manager : homebrew (download and install from : [https://brew.sh/](https://brew.sh/))
3. Ruby : (installation : `$ brew install ruby`)
4. Rails :  (installation : `$ brew install rails`)
5. Postgres : postgresql-10.3.high_sierra (installation : `$ brew install postgresql`)


## Database settings with Postgres 

`warning` : the following settings are done with the current OSx user 

Check `postgres` status (if any issue, please check `homebrew doctor`):
```
$ brew info postgres
```

Start the service for the first time:
```
$ brew services start postgresql
```

Set up the path to store the DB file system and the super user:
```
$ initdb /usr/local/var/postgres
```

Check your current user login in OSx: 
```
$ whoami
```

Set your password with the `psql` tool:
```
$ psql
```

Run the `\password` command to set your password:
```
<login>=# \password
Enter new password:
Enter it again:
<login>=#
```

Exit from the `psql` interface with the command `\q`. Initiate the database with your username as DB name:
```
$ createdb <login>
```

Check that the DB is initiated correctly:
```
$ psql -l
```

Expected output:
```
                              List of databases
   Name    | Owner | Encoding |   Collate   |    Ctype    | Access privileges
-----------+-------+----------+-------------+-------------+-------------------
 <login>   |<login>| UTF8     | en_US.UTF-8 | en_US.UTF-8 |
```

# PIA back end installation

Let's work in the local folder `~/app` from now. Clone the `pia-back` repository from github:
```
$ git clone https://github.com/LINCnil/pia-back.git
```

Go to the `pia-back` folder:
```
$ cd pia-back
```

Change the local version to `1.6.0` using the tag:
``` 
$ git checkout tags/1.6.0
```

Copy the dabatabse settings template :
```
$ cp config/database.example.yml config/database.yml
```

Fill the fields `username` and `password` with the PostgreSQL username and password created in the previous step. Copy the application settings template :
```
$ cp config/application.example.yml config/application.yml
```
Run the command `rake secret` and paste the secret key in the file to fill the key `SECRET_KEY_BASE`. Install all dependencies:
```
$ bundle install
```

Create database:
```
$ bin/rake db:create
```
Expected output:
```
Running via Spring preloader in process 83020
Created database 'pia_development'
Created database 'pia_test'
```

Create tables
```
$ bin/rake db:migrate
```

Run the test
```
$ bin/rake
```

Expected output:
```
Running via Spring preloader in process 83139
Run options: --seed 21410
# Running:
.............................
Finished in 0.890501s, 32.5659 runs/s, 44.9185 assertions/s.
29 runs, 40 assertions, 0 failures, 0 errors, 0 skips
```

# Run the application

Start the web server without option:
```
$ bin/rails s 
```

The application runs on this address : `http://localhost:3000`. To change IP and port, use the options `-b` to change the IP and `-p` to change the port, for example:
```
$ bin/rails s -b 192.168.0.10 -p 8080
```

Well done the PIA back end is up and running. Check the next post to set up the Front End application [here](/2018/03/05/Get-PIA-app-from-srcatch-p2) 
