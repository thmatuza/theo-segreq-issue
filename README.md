# THEOplayer 2.69.1 SegmentTimeline issue reproduction project

I found that THEOplayer 2.69.1 cannot play a DASH stream under certain conditions.
Here is conditions.
1. Live manifest
2. SegmentTimeline is used
3. Period@start contains third non zero number after the decimal point.
  + OK: `PT1587091109.080000S`
  + NG: `PT1587091109.008000S`

When it cannot play, it requests wrong segment URL.
In this project, it suppose to send next segment URL.
```
https://dash.akamaized.net/dash264/TestCases/1c/qualcomm/2/BBB_720_1M_video_491520.mp4
```

However, it sends URL with 491519(491520 - 1).
```
https://dash.akamaized.net/dash264/TestCases/1c/qualcomm/2/BBB_720_1M_video_491519.mp4
```

The server returns 404 error.

## How to run

```sh
$ cp [your certificate] ./cert/server.crt
$ cp [your private key] ./cert/server.key
$ pip install Flask
$ python application.py
```

Play `https://[your domain]:8443/manifest.mpd` with THEOplayer 2.69.1

You will see it doesn't start playing.

Next, comment out next line on application.py

```application.py
unixtime += 0.001
```

Play the same url and you will see it start playing.
