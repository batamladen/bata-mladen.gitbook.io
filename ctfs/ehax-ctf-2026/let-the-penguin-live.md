# let-the-penguin-live

**Category:** Forensics

***

### Challenge Description

> In a colony of many, one penguin's path is an anomaly. Silence the crowd to hear the individual.

We were given a `challenge.mkv` file.

***

### Solution

Inspect the MKV streams

Running `ffprobe` revealed the MKV contained three streams — one video and **two audio tracks**, both FLAC stereo:

```bash
ffprobe -v quiet -print_format json -show_streams challenge.mkv
```

* Stream 0: H.264 video
* Stream 1: FLAC audio — `"English (Stereo)"` (default)
* Stream 2: FLAC audio — `"English (5.1 Surround)"` (but actually also stereo — suspicious!)

We also spotted a decoy flag in the MKV metadata:

```
COMMENT : EH4X{k33p_try1ng}
```

#### Extract both audio tracks

```bash
ffmpeg -i challenge.mkv -map 0:a:0 track1.flac
ffmpeg -i challenge.mkv -map 0:a:1 track2.flac
```

Since this is based on the penguin meme that goes for the mountains and does not follow the other group of penguins we guess there is an anomaly btw the two tracks since they both are similar (the audio is 99% the same, so we search for the anomaly next).

#### Compute the difference between the two tracks

We used Python with `soundfile` and `numpy`:

```python
import numpy as np
import soundfile as sf

d1, r1 = sf.read('track1.flac')
d2, r2 = sf.read('track2.flac')

min_len = min(len(d1), len(d2))
d1, d2 = d1[:min_len], d2[:min_len]

diff = d2 - d1
diff = diff / np.max(np.abs(diff))

sf.write('diff.wav', diff, r1)
```

diff.wav

#### Spectrogram analysis

We generated a spectrogram of the difference signal using SoX:

```bash
sox diff.wav -n spectrogram -o diff.png
```

This revealed a brief signal burst around the 25-second mark we first listened to it in slow-mo to try hear the flag, but since it was gibberish only reasonable thing to do was zoom in the spectrogram for better resolution of the represented noise.

```bash
sox diff.wav -n spectrogram -o diff_zoom.png -S 24 -d 4 -x 2000 -y 1000
```

The flag was clearly visible as **text encoded in the spectrogram** — the hidden audio track contained a signal whose frequency pattern spelled out the flag visually.





<figure><img src="../../.gitbook/assets/diff_zoom2.png" alt=""><figcaption></figcaption></figure>

***

### Flag

```
EH4X{0n3_tr4ck_m1nd_tw0_tr4cK_f1les}
```
