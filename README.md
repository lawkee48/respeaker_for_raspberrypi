ReSpeaker for Raspberry Pi
==========================

The repository contains some examples to use ReSpeaker series mic arrays on Raspberry Pi.

The examples are included in [the custom image](https://v2.fangcloud.com/share/7395fd138a1cab496fd4792fe5?folder_id=188000207913&lang=en) with pre-installed `seeed-voicecard`, `snowboy`, `voice-engine` and etc. You can flash the custom image to get started easily.

### Hardware
ReSpeaker 2 Mic Hat, ReSpeaker 4 Mic Array or ReSpeaker 6 Mic Array (they are all pi hats)

# Getting Started with Raspberry Pi
## Driver installation and configuration

1. Connect ReSpeaker 2-Mics Pi HAT to Raspberry Pi (Careful with the orientation)

2. Setup the driver on Raspberry Pi
- 2.1. Get the Seeed voice card source code, install and reboot.
```
sudo apt-get update
cd ~/Downloads/
git clone https://github.com/respeaker/seeed-voicecard.git
cd seeed-voicecard
sudo ./install.sh
sudo reboot now
```
- 2.2. Check that the sound card name matches the source code seeed-voicecard by command `aplay -l` and `arecord -l`.

- 2.3. Configure sound settings and adjust the volume with **alsamixer**, [alsamixer](https://en.wikipedia.org/wiki/Alsamixer) is a graphical mixer program for the Advanced Linux Sound Architecture (ALSA) that is used to configure sound settings and adjust the volume. 
```
alsamixer
```
Press **F6** to select sound card of seeed-voicecard device, increase to a resonable volume of headphone and speaker. Quit the program with **ALT+Q**, or by hitting the **Esc** key.

- 2.4. Test, you will hear what you say to the microphones (don't forget to plug in an earphone or a speaker):
```
arecord -f cd -Dhw:2 test.wav
aplay -Dhw:2 test.wav
```
**Note**: **-Dhw:1** is the recording(or playback device number), depending on your system this number may differ (for example on Raspberry Pi 0 it will be 0, since it doesn't have audio jack).

## Usage Overview
To run the [ReSpeaker 2-Mics Pi HAT examples](https://github.com/respeaker/mic_hat.git), clone repository to your Raspberry Pi
```
cd ~/Downloads/
git clone https://github.com/respeaker/mic_hat.git
```
All the Python scripts, mentioned in the examples below can be found inside this repository. To install the necessary dependencies, from mic_hat repository folder, run
```
sudo apt-get install portaudio19-dev libatlas-base-dev
cd mic_hat/
pip3 install -r requirements.txt
```

- APA102 LEDs

Each on-board APA102 LED has an additional driver chip. The driver chip takes care of receiving the desired color via its input lines, and then holding this color until a new command is received.
```
python3 interfaces/pixels.py
```

### Software
+ snowboy for KWS (Keyword Search / Keyword Spotting)
+ webrtc audio processing for NS (Noise Suppression)
+ speexdsp for AEC (Acoustic Echo Cancellation)
+ GCC-PHAT for DOA (Direction Of Arrial)
+ avs for alexa voice service
+ voice-engine for connecting all the elements together


```
pip install webrtc-audio-processing speexdsp voice-engine avs
```
and go to [kitt-ai/snowboy](http://github.com/kitt-ai/snowboy) to install snowboy.



### Files

----------------------------------------------------------

```
├── 2mic                               # ReSpeaker 2 Mic Hat
│   └── ns_kws_doa_alexa.py                hands-free alexa with NS, KWS and DOA (0 ~ 180 degree)
├── 4mic                               # ReSpeaker 4 Mic Array
│   ├── kws_doa.py                         KWS and then DOA
│   ├── ns_kws_doa_alexa.py                hands-free alexa with NS, KWS and DOA (0 ~ 360 degree)
│   └── ns_kws_doa.py
├── 6mic                               # ReSpeaker 6 Mic Array
│   ├── aec_ns_kws_doa_alexa.py            hands-free alexa with AEC, NS, KWS and DOA (0 ~ 360 degree)
│   ├── aec_ns_kws_doa.py                  has 2 loopback channels for AEC
│   └── kws_doa.py
├── ns_kws_alexa.py
├── ns_kws.py
└── ns_vs_raw.py                       Compare KWS between raw audio and audio with NS
```

Reference:
- ReSpeaker -Getting Started with Raspberry Pi. https://wiki.seeedstudio.com/ReSpeaker_2_Mics_Pi_HAT_Raspberry/
