# Simple Typescript Clock

A simple, maybe not so simple clock replacement for the office of ACM@UIC.

## Features

* Time and Date
* CTA Bus and Train Arrival Times
* Weather from DarkSky

## Using with docker

1. `docker` and `docker-compose` should be installed.
2. Download just the `docker-compose.yaml` file or clone the repository.
3. Create a `.env` file based on `.env.example`.
4. Run `docker-compose up -d` (Run `docker build -t bmiddha/simple-js-clock:latest` before to include local changes).

`docker run` command
```
docker run -d -p 8080:8080 -e HTTPS="false" -e KEY="/root/ssl/ssl.key" -e CERT="/root/ssl/ssl.crt" -e PORT="8080" -e CTA_TRAIN_API_KEY="" -e CTA_BUS_API_KEY="" -e METRA_TRAIN_ID="" -e METRA_TRAIN_SECRET="" -e DARK_SKY_API_KEY="" -e COLORS="303030 01579B 006064 304FFE 004D40" -e BUS_STOPS="6700 6627 307 332 4640 14487 6347 206" -e TRAIN_STATIONS="40350" -e WEATHER_LAT_LONG="37.8267,-122.4233" -e UNITS="both" bmiddha/simple-js-clock
 ```

## Installation

1. Clone the repository.
2. Navigate to the repository directory.
3. Create a `.env` file based on `.env.example`.
4. Run `npm install` to install dependencies.

## How to use

1. Run `npm run start` to start the npm server or `npm run watch` to watch for changes.
2. Navigate the browser to `locahost:8080` or the port specified in the `.env` config.

## Offline Mode

Offline mode will disable api requests to the server leaving only the clock running. It can be activated by going to `/offline` path.

## Demo Mode
Demo mode works like offline mode but displays demo information instead of real data from apis. It can be activated by going to `/demo` path.

## Configuration
To override the default config, you can use the URL GET parameters or by pressing `c` to open the config options.

# Deployment on a Raspberry PI

## Step 0: Install Packages
```
sudo apt update && sudo apt upgrade -y
sudo apt install --no-install-recommends -y chromium-browser xserver-xorg x11-xserver-utils xinit
```

## Step 1: Enable auto-login
Use the `raspi-config` tool to enable auto-login to the console.
`sudo raspi-config` => Boot Options => Desktop / CLI => Console Autologin

## Step 2: Set console orientation
Edit the file `/boot/config.txt`
Add the following option:
```
display_rotate=3
```

```
display_rotate=0 Normal
display_rotate=1 90 degrees
display_rotate=2 180 degrees
display_rotate=3 270 degrees
```

## Step 4: `.xinitrc` file

`/home/pi/.xinitrc`
```
#!/bin/sh
xset -dpms
xset s off
xset s noblank

xrandr --output default --rotate left

setxkbmap -option "terminate:ctrl_alt_bksp"

unclutter &
chromium-browser http://server:8080 --window-size=1080,1920 --start-fullscreen --kiosk --incognito --noerrdialogs --disable-translate --no-first-run --fast --fast-start --disable-infobars --disable-features=TranslateUI --disk-cache-dir=/dev/null
```
Replace `http://server:8080` with nodejs server.

## Reboot

# Deployment on NixOS

## Step 0: Provision machine

This project follows a standard NixOS deployment process. Utilize the [`nixos/configuration.nix`](./nixos/configuration.nix) file to deploy a standard NixOS installation on a given machine. This sets up [cage](https://github.com/Hjdskes/cage) which is a Wayland kiosk application. The configuration is setup to launch Google Chrome and visit a local dockerized instance of the project.

## Step 1: Build the application

Build the application locally.

```bash
docker build -t acm-uic/simple-ts-clock:latest .
```

## Step 2: Setup environment secrets

Make a copy of the example `.env` file and populate the variables with the ones used for your setup.

```bash
cp .env.example .env
nvim .env
```

## Step 3: Run Docker-compose

Run the application using docker-compose.

```bash
docker-compose up -d
```

# Authors

This project was originally a rewrite of [sudoclock](https://github.com/acm-uic/sudoclock) by [clee231](https://github.com/clee231).

The project has since been rewritten mostly in the [simple-js-clock](https://github.com/bmiddha/simple-js-clock) repo by [bmiddha](https://github.com/bmiddha) and other contributors.

In January 2023, the simple-js-clock repo was forked to continue development here.
