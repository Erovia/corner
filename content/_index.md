# Erovia's QMK Corner

Some of you may know me from the subreddit or the QMK Discord. I'm the clueless fella who tries to help others regardless.


My plan with this site is to compliment the [official QMK Documentation](https://docs.qmk.fm).  
This site has **no** trackers, so let me know if you found it to be useful. :)

### Things I'm working on...

{{% notice note %}}
No promises, this more like to motivate myself to get around doing these.  
Also, this is not an exhaustive list.
{{% /notice %}}

1. [Reviving the Planck THK](https://github.com/Erovia/qmk_firmware/tree/planck_thk_revived)

   I first heard about this keyboard when a community member tried to flash his board and asked for help.  
   Sadly, the code for this board was never merged and bitrot to the point of being incompatible with modern QMK.  
   Trying to help, I've managed to forwardport the code, so it compiled and booted. Or rather I think it booted, as according to the user it played the startup sound, but typing did not work.  
   I'd like to finish the work, but further debugging will require a kit, so if you have one (preferably unbuilt), let me know. ;)

   **Update**: Thanks to Jack, I got my hands on the hardware. I have it working, the code will just needs some finishing touches.
    I also plan on creating a [build log](posts/thk), to document everything.

1. [VIA keymap creation from CLI](https://github.com/qmk/qmk_firmware/pull/8353)

   The plan is to generate most of the things needed for adding VIA support with a simple command.  
   Ideally, only the layout information will needed to be created by hand with KLE.
