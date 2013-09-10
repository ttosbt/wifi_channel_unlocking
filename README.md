# wifi_channel_unlocking

Damn the man! Sometimes you just want to scan all of those delicious frequencies that your hardware allows you to do, but the darn regulations always get in the way...well, not any more.

Provided are all the tools necessary to load and use a modified version of the wireless-regdb file `'db.txt'` so you can easily include a new region with no disabled wifi channels.  With this you can also modify channel bandwidth, and maximum power output per channel.

Be careful however, using channels or transmission powers not allowed in your region could result in some spooks knocking on your doors or a fine at worst, and/or a fried wireless radio at best.  Stay within your hardware specifications and regional laws.

I am not responsible for what you do with your newly gained education, period.  :^)

## Detailed Description & Usage

When you run the `'add_reg.bsh'` script it will first build wireless-regdb which creates a new `'regulatory.bin'` based on the included modified version of `'db.txt'` and generates some keys.  The new `'regulatory.bin'` file based on your systems type will be placed in `'/usr/lib/crda'` or `'/usr/lib64/crda'`.  However, before you can use the new region, the script needs to use the private key that was generated in the build of wireless-regdb to install a new [CRDA](http://wireless.kernel.org/en/developers/Regulatory/CRDA) which checks the validity of, you guessed it, `'regulatory.bin'`.  With the newly built and installed CRDA the cfg80211 module can be reloaded or the system rebooted so you can use the new region.

This repository contains the [wireless-regdb (2009.11.25-1)](http://wireless.kernel.org/download/wireless-regdb_2009.11.25-1.tar.gz), [crda (1.1.3)](http://wireless.kernel.org/download/crda/crda-1.1.3.tar.bz2), and a few scripts to automatically build and install a new `'regulatory.bin'` file on most Linux distributions.

## Provided Scripts

`'add_reg.bsh'` - Used to build and install a new `'regulatory.bin'` along with the necessary signing authorities so your system can trust the new file.
`'remove_reg.bsh'` - Used to put your system back to the way it was before you added a new region.  NOTE, this does not uninstall CRDA, it will remain at the 1.1.3 version.

I was not able to get the unload and loading of the cfg80211 module to work properly, so a reboot will do nicely.  If anyone knows how to fully unload and reload the necessary modules to get this working without a reboot, please let me know!)

Once you have sucessfully loaded the new region you can run the followig commands to activate the new region and verify power levels:

iw
iwconfig
iwlist

## db.txt Format Tutorial

I have included a modified `'db.txt'` within wireless-regdb the modified version contains a new region named HX and is described as follows:

country HX:
       (4910 - 6090 @ 40), (N/A, 27)
       (2402 - 2494 @ 40), (N/A, 27)

The format of this new region is:

country <new country code>
       (<bottom channel> - <top channel> @ <maximum channel bandwidth>), (N/A, <maximum transmit power>)

As you can see I set the maximum transmit power to 500mW (27dBm), this should be fairly low enough to not damage most cards. If you want to boost the maximum power you can change that to 30dBm (1000mW) or 33dBm (2000mW). Then change the transmit power on your card using:

`'iwconfig <interface> txpower <new power level>'`

Then check to see if it actually worked:

`'iwconfig <interface>'`

## Testing

The scripts and source environments provided above were tested and built on a system running BackTrack5 R3.  I was able to use all of the channels in the 2.4Ghz and 5.0Ghz bands (provided the hardware was capable) on the following pieces of hardware and at the specified transmit power:

AWUS051NH @ 1000mW
AWUS036NH @ 2000mW
AWUS036H @ 1000mW
WLI-CB-G54S @ 2000mW

I am pretty sure other cards will work, but again stay within the specifications of your hardware.  Again, I am not responsible for any damage you may cause to your hardware.

## Credits

The bulk of the procedure that the scripts and new region was based on came directly from [Rick Deckhardt](http://deckhardt.nl/blog/2011/01/20/regulatory-limitations-in-linux-wireless/), so thanks Rick!  :^)

Everything else including the scripts were written and are provided for use as you see fit solely by me.
