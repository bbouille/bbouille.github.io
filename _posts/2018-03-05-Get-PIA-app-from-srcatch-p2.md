---
title: Get PIA app from scratch on OSx part 2 (Front End)
layout: post
permalink: 2018/03/05/Get-PIA-app-from-srcatch-p2/
tags: [PIA, angular]
---
Set up the Front End app on OSx

# Introduction

This post describes the main steps to install and run the PIA front end application.
It follows the post about PIA back end application. Please read it first [here](/2018/03/03/Get-PIA-app-from-srcatch-p1).

# Prepare your environment

## Assumptions 

1. You've have successfuly deployed the PIA back end application and it's running on `http://localhost:3000`
2. You're testing the application in a development environment - NO PRODUCTION USE


## Environment settings 

The following configuration is used to review this application.

1. OS : OSx 10.13.3
2. Package manager : homebrew (download and install from : [https://brew.sh/](https://brew.sh/))
3. npm : (Download and run `macOS Installer (.pkg)` from [https://nodejs.org/en/download/](https://nodejs.org/en/download/))


# PIA front end installation

Let's work in the local folder `~/app` from now. Clone the `pia` repository from github:
```
$ git clone https://github.com/LINCnil/pia.git
```

Go to the `pia` folder:
```
$ cd pia
```

Change the local version to `1.6.0` using the tag:
``` 
$ git checkout tags/1.6.0
```

Update `npm` tool:
```
$ npm install npm@latest -g
```

Get the Angular CLI:
```
$ npm install -g @angular/cli
```

Install the `node` modules:
```
$ npm install
```

Build the application (development version):
```
$ ng build
```


# Run the application

Start the web server without option:
```
$ ng serve
```

Last but not least, navigate to [http://localhost:4200/#/settings](http://localhost:4200/#/settings) and setup the URL of the back office application (default: `http://localhost:3000`). You can access the application from your browser: [http://localhost:4200](http://localhost:4200)
