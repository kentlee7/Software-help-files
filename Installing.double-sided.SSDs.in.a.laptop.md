
# Installing double-sided SSDs in a laptop 


Don't fret if your SSD doesn't seem to work 


I recently ordered a new laptop, an Asus ProArt, and it's a beautiful little beast of a laptop. The weight is about 1.7 kg or less, so for such a robust, spec'd-out machine, it is not that heavy. It came with one 2 TB NVMe SSD, with Windoze 11 preinstalled. Not my favorite operating system, but one that I need for some of my work. 

I wanted to also install Linux on it to make a dual boot machine. So I ordered an 8 TB NVMe SSD for the second drive bay in the laptop. That would allow me to install Linux on the second SSD, with plenty of room for video editing, files for my YouTube videos, and running virtual machines. 

So I ordered a Western Digital Black SN850X in the 8 TB size. Now some newer NVMe’s come with heatsinks, which obiously will not fit inside a laptop, so that’s something to pay attention to if you are ordering an NVMe for your laptop. Of course, I ordered the version without the preinstalled heat sink.

I opened up the back of the laptop, inserted the 8 TB NVMe in the second M.2 drive bay, put the back cover on, and booted to a Linux USB to install EndeavorOS. The Linux installer did not detect the new SSD; nor did GParted. I booted to BIOS / UEFI, and saw that there the new drive was also not detected. I was worried...

# Dealing with 2x SSDs
After some searching I found a likely source of the problem. I did not pay attention to the fact that the WD Black SN850X is a double-sided SSD, and this was my first time to use a double-sided SSD. Apparently a problem with such drives in laptops is that the second side of the NVMe hits grounding pins or metal components of the motherboard. This apparently shorts the NVMe's connection. 

The recommended solution is to put Kapton tape on the second side of the NVMe to prevent contact between the drive and the motherboard.  Electrical tape might work, according to what I read, but probably as a short-term solution, as it is not so heat resistant.  

Kapton is an insulating tape, generally translucent orange in color, which is used in electronics. It insulates electrical components, and resists heat well. In electronics, it can apparently last a long time. I saw it once inside an old laptop that I took apart, and thought it might be nice to have some. So I ordered it, but never had a chance to use it. Until now. 

Kapton comes in different widths. It can be slightly expensive, say, $7-12 per roll, but it's worth having around. My Kapton tape was only 5 mm wide, but it worked. I cut several strips, and completely covered the underside of the NVMe. Note, of course, that the tape should not block the connector pins at the end of the NVMe. You just need to cover the underside that might touch the motherboard. 

I closed the case, and since one layer of tape was enough, closing the laptop cover should not be a problem. After booting, the new NVMe was detected in BIOS / UEFI, and it was detected by the Linux installer. I was able to successfully install Linux onto the second drive. 

Now for a dual boot laptop, it might be necessary to install Grub or another bootloader onto the EFI partition of the primary SSD / NVMe, in order to choose which operating system you want to boot into, and it may be necessary to install a bootloader afterwards if the Linux installer does not do so. Otherwise, when you reboot, it will automatically boot into Windows on the primary drive. 

By the way, I also wondered if a piece of the material from anti-static bags that computer components come packed in would work for this. But as I read on it, I found out that these bags of a layer of polyethylene, which is designed to conduct and dissipate static charges, so that material would not have worked. Just order some Kapton tape, as it's not hard to find online. 


# Conclusion
So laptops can handle double-sided NVMe drives, but some Kapton tape is probably needed. If that does not work, then you may need to fret. 

And if you wonder why your techie friend, boyfriend, or husband likes to keep a lot of parts that you think s/he should just throw away, this is why. (I once threw away my old DVI cables, and then my wife's old monitor and graphics card required one, so I had to buy a new one; lesson learned: never throw away computer supplies or parts.) Well, I bought my Kapton tape years ago, and did not have a real use for it until now. So I'm glad I had it. 

