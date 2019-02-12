# 360 Video Pipeline

I got my first 360 camera to record a vlog about making the exoskeleton from Elysium, but quickly realized the medium's potential and set out to capture as many experiences as possible.

To complete my first project, a 45-minute show of my favorite local band, I had to build a 360 video pipeline along with a render farm. I hope that this article can help other 360 videographers improve their delivery pipeline.

## Overview

(static illustration)
* Capture
* Stitch
* Mix & Master Audio
* Transcode 1: Color Grade, Warp, Re-sample
* Transcode 2: De-noise, Re-sharpen, Re-sample
* Animate Titles
* Encode 360
* Encode 2D
* Upload

## Equipment

I use Insta 360 Pro I with an external SSD and battery. I leave the stock battery and a memory card inside the camera so I can switch to them if the external ones fail.

The onboard mic cannot handle the loudness and dynamic range of a real performance so I supplement the sound with an external stereo sound recorder and the audio from the venue board recorded on to a USB drive.

## Pre-flight

I check the equipment the day before and 45 minutes before filming:

* Camera, external sound recorder, and light batteries must be charged.
* Camera and sound recorder must have their media formatted and a few minutes of test footage recorded to ensure integrity.
* Camera should have redundant storage and power.
* Camera, sound recorder, lights, monopod, and a USB stick for recording audio from the venue board should be packed along with accessories.

## Setup

I follow this checklist to setup at the venue:

* Unpack the camera and set it up on its monopod.
* Unpack and setup the audio capture: set level, activate limiter, plug in headphones and start recording. I recommend against using a leveler because it turns up the ambient noise when the performance gets quiet.
* Turn on the camera and configure settings as described in the next section, selecting external SSD as the media for recording.
* Work with the artist and the venue to find an optimal position for the camera so that it looks unobtrusive on the stage yet captures a scene that makes the viewer feel like they are a part of the performance. Avoid placing the camera where it might cast a shadow on the performer(s).
* Rotate the camera on its monopod to place the stitch lines that occur in-between lenses in unimportant areas that artists on stage will not be crossing often or standing nearby.
* Unpack and setup the lights.
* Work with artist and venue to find an optimal position for the lights.
* Start the camera recording.
* Verify levels on the external sound recorder during soundcheck, just before the show.

### Camera Settings

I use the following settings for Insta 360 Pro I in Video mode:

|Setting|Value|
|-------|-----|
|**General**||
|Mode|Normal|
|Content type|360&deg; Pano|
|Real-time Stitching|off|
|Save origins from six lens|on|
|Single lens resolution|8K@30F|
|Flat color mode|on|
|Audio gain|-24 dB|
|**Exposure**||
|Mode|Manual|
|ISO|640|
|Shutter|1/40s|
|WB|Auto|
|**Property**||
|Brightness|0|
|Saturation|64|
|Sharpness|3|
|Contrast|64|

## Stitch Settings

I use the Insta360 Stitcher with the following settings:

|Setting|Value|
|-------|-----|
|**Stitch**||
|Content Type|Monoscopic|
|Stitching Mode|New Optical Flow|
|Sampling Type|Slow|
|Blender Type|Auto|
|Use original offset|off|
|Use Default Circle Position|on|
|Gyroscopic Stabilization|off|
|Use Hardware Decoding|6|
|Use Hardware Encoding|off|
|Software encoding speed|Highest Quality (Slowest)|
|Use nadir logo|off|
|**Reference Frame**|(default setting)|
|**Cut Video**||
|From|(select 30 seconds before the artist performs)|
|To|(select 30 seconds after the artist performs)|
|**Output**||
|Resolution|8K|
|Output Format|MP4(H.264/H.265)|
|Codec Type|h264 codec|
|Profile|High|
|Bitrate|350 Mbps|
|Frame Rate|30 fps|
|Audio Type|Normal|
|Output Directory|(select your fastest drive that can fit at least 500 GB)|
|Output File Name|(projectname-stitch.mp4)|

After starting the batch stitching process I switch tabs to "Pro 2 File Manager" to continue the stitching but hide the preview image. Leaving the preview up crashes the stitcher.

## Mixing and Mastering Audio

I work on the sound next as any performance should be enjoyable just by listening like it was on a radio:

* Create a new Premiere Pro project.
* Import stitched footage and place into "stitch" bin.
* Create a new sequence from footage.
* Clip the footage to the exact beginning of the performance, and leave a few seconds after the performance to capture applause and the performer(s) thanking the audience.
* Import audio from the external recorder into the "audio" bin.
* Import audio from the board into the "audio" bin.
* Place any imported audio tracks into the stitch sequence.
* Sync any imported audio tracks to audio from the camera's onboard mic by using the Premiere Pro sync audio feature, or manually if that doesn't work.
* Mute the camera audio track.
* Mix the external audio sources, if more than one. If the audio from the board sounds great, the ambient mic recording can be discarded.
* Apply fade automation to the beginning and end of the video by using Easing curves, and add any necessary cross-fades between audio sources.
* Apply one or more multi-band compressors if necessary to repair a mix with a bad spectrum balance.
* Apply a loudness maximizer if necessary, and choose a setting to maximize left/right channels independently to ensure both channels are balanced in volume.
* Apply a brickwall limiter to the entire audio track at -4 dB. The AAC audio codec that will be used for the final video works best when the audio does not peak too close to 0 dB.
* Render audio for the entire sequence and import back into the project, also placing into the "audio" bin.

## Transcoding

I transcode the footage twice, downscaling each time to average the "noise" pixels because this lowers the noise floor on the image and makes it look sharper but without accentuating noise like a sharpening algorithm would, or blurring like a de-noising algorithm would. I also apply different plug-ins on each pass.

I output both passes to PNG sequences, but using an uncompressed format like BMP may save up to 60% of time on rendering each pass including the final encode, if you have have several TB of space available. Note that for each transcoding or encoding operation, input and output files should not be on the same drive to eliminate I/O bottleneck.

Image sequences are preferred because Adobe Media Encoder crashes every few hours, and outputting each frame as a separate file will let you restart the process without loosing processed frames.

## Transcoding - First Pass

In this pass I perform color grading, rotate the camera, and re-sample the result from 8K to 5.2K.

### Color Grading

I shoot in Flat Color mode with lower contrast and neutral settings to retain detail in colors and shadows. If a higher contrast or saturation are applied when shooting, pixel values in each channel are limited by chopping off highs and lows, which cannot be recovered.

When this information is retained, applying a curve during color grading lets you re-distribute shadows and highlights and apply your own limiting to cut off values that contain more noise and retain values with detail.

I toggle the following transforms in different combinations to determine if they are needed and compare side-by-side:

* **Exposure**: This can be pushed to increase the richness of shadows, but then it would require more de-noising which blurs details.
* **Contrast**: This can be used along with Curves to tune the overall contrast. It adds an S-curve over the entire spectrum, with the sharpness of the "S" controlled with this slider. That extra curve is then mixed in with the master Curve.
* **Shadows**: Just like Contrast, this control is a shortcut that can be used to control the amplitude of shadow frequencies in the image without making a complex Curve. This creates a curve with a dip or a peak on shadows and mixes it in with the Contrast and Master curves.
* **Vibrance**: I usually increase this to around 16 to enhance colored lights. Spot-check your footage for any places where this might blow out colored highlights. I don't use this setting for scenes without colored lights due to its "compress and apply makeup gain" effect that pulls out highly visible, animated noise.
* **White balance**: A proper white balance often ruins the mood established by colored lighting, so I would only apply this if the main lights in the scene are tungsten or daylight balanced.
* **Curve**: The master color grading curve. I don't mess with curves for each channel because that's equivalent to adjusting the white balance, however adjusting the channel curves is a great surgical substitute for the White Balance picker. When color grading Flat Color footage, the Master curve will resemble an "S" shape that re-maps the flat color distribution back into realistic colors, letting you choose how much contrast you want to bring back, and how you want to distribute shadows and highlights.

Note that with the **camera settings** listed in the previous section (Brightness 0, Contrast 64), unless you reduce Contrast further to capture more dynamic range it's unlikely that any color grading will be required other than bumping up Vibrance.

### Warping

Choosing the "initial" view seen by the viewer before they rotate the screen is accomplished by warping, or re-mapping the pixels across the video sphere. This operation should be done in the Stitching phase, but the Insta 360 Stitcher does not let you specify precise values in degrees so I have to do this with the VR Rotate Sphere plug-in as part of the first transcoding pass.

### Re-sampling

## Transcoding - Second Pass

### De-noising

### Re-sampling

## Animating Titles

## Encoding 360 video

## Encoding flat video

## Uploading

### Facebook

### YouTube

### Google Drive

## Building a Render Farm

## Equipment List
