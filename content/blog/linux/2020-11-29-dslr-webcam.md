---
title: 2020-11-29
date: 2020-11-29 12:00:00
logo: ../../images/blog/linux/2020-11-29-dslr-webcamNEW.jpg
categories:
  - rootpages
tags:
  - dslr
  - notes
  - obs
  - rootpages
---

[Back to Linux blog.](../#linux)

# Using a DSLR as a Webcam on Linux

{{< image src="../../images/blog/linux/2020-11-29-dslr-webcam.jpg" alt="DSLR Laptop GNU Linux Penguin Microphone" set="fit" >}}

Ever wonder how to setup a DSLR to use as a high-end webcam in Linux? Me too.


I had a spare camera that I used to use all the time. That is, until my wife got a Google Pixel phone. It has been collecting dust for years - until now! I have been inspired by my peers and even my wife who are striving for that ultra-professional look during work calls. Using a DSLR really takes your video call to the next level.


For Windows users, it is *almost* a plug-and-play experience with [Canon](https://petapixel.com/2020/11/11/canon-officially-launches-eos-webcam-utility-software-for-macos-and-windows/), [Nikon](https://petapixel.com/2020/08/06/nikon-unveils-free-software-that-turns-your-camera-into-a-webcam/), and [Sony](https://petapixel.com/2020/08/20/sony-unveils-free-software-that-turns-your-camera-into-a-webcam/) now having software to convert their DSLRs into webcams. Some vendors support macOS but good luck finding any support for Linux users! Add this to the list of "things you need to get your hands dirty with to have it work with Linux." Although, I am proud to say that that list is ever shrinking! Year of the Linux desktop coming, anyone?


This tutorial focuses on Arch Linux and should also apply to Ubuntu and openSUSE. For Fedora users, you need to compile the obs-v4l2sink from source using a [workaround patch](https://github.com/CatxFish/obs-v4l2sink/issues/42#issuecomment-678714048) or wait until OBS Studio 26.1 gets fully released in the coming days/weeks.


My full guide on setting up a DSLR for use in video conferencing can be found in my technical notes [here](https://github.com/ekultails/rootpages/blob/master/src/computer_hardware/webcams.rst#videos).


P.S. - Pro tip for glasses wearers from my buddy [Kevin Carter](https://cloudnull.io/): get a polarizing filter to help reduce the amount of glare from the computer screen on your glasses. Also try changing the position of your glasses, rearranging lights, wearing contacts, or just go get LASIK surgery. ;-)
