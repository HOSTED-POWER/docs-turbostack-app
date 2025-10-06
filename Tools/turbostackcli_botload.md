---
order: 200
icon: robot
---
# TurboStack CLI Tools - Botload
We provide a *botload* tool that grants you more insight in your nginx / apache logs.
This is how you use it via our TurboStack CLI.
## Features
 - List your logfiles and their size.
 - Show the amount of requests, and bot percentage.
 - Top 10 most common bots.
 - Top 10 IP addresses.
 - Hourly breakdown of requests.
 - Breakdown of requests (GET, POST, ...etc). 
### Functions
Normal usage (list and pick a log to analyse):
```
tscli tools botload list
```
Search for a specific IP in all logs:
```bash
tscli tools botload ip
```
Live view of a specific log.
```bash
tscli tools botload live
```
Compact overview for shared packages.
```bash
tscli tools botload shared
```
Search between two given timeframes.
```bash
tscli tools botload list --time 12:00:00 17:00:00
```
---
# Examples

The following, is an example of the main features. Take in mind that bigger logs, might take a couple of minutes. If the server has a high load, it will add on to the analysis time.
```
Detected Nginx – logs in /var/log/nginx

Available logs:

 1) access.log            0.3 MB
 2) access.log.1          0.2 MB
 3) error.log             0.0 MB
 4) error.log.1           0.0 MB
 5) prod.log             41.2 MB
 6) prod.log.1           34.3 MB
Select log (number or filename): 5

Analysis of prod.log:
Total requests:    141261
Crawler requests:  15880  (11.24%)

Top 10 bots:
     18636 bingbot
      5735 robot
      5443 ahrefsbot
       864 amazonbot
       263 semrushbot
       246 dotbot
       195 googlebot
        43 clarity-bot
        20 petalbot
        15 facebookexternalhit

Top 10 IPs:
      1478 2a03:2880:f800:13:: (crawler)
      1423 2a02:348:91:63c7::1
      1405 2a03:2880:f800:f:: (crawler)
      1387 2a03:2880:f800:3:: (crawler)
      1382 2a03:2880:f800:29:: (crawler)
      1379 2a03:2880:f800:36:: (crawler)
      1373 2a03:2880:f800:e:: (crawler)
      1373 2a03:2880:f800:44:: (crawler)
      1371 2a03:2880:f800:7:: (crawler)
      1360 2a03:2880:f800:1e:: (crawler)

Hourly breakdown:
  00:00  1573
  01:00  1823
  02:00  2577
  03:00  1408
  04:00  1133
  05:00  1552
  06:00  4698
  07:00  7296
  08:00  7951
  09:00  7522
  10:00  10518
  11:00  7106
  12:00  7544
  13:00  6948
  14:00  8651
  15:00  7419
  16:00  7348
  17:00  7146
  18:00  6872
  19:00  6987
  20:00  7718
  21:00  7203
  22:00  7240
  23:00  5028

Request methods:
    141189 GET
        53 HEAD
        18 POST
```