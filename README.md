# example gentoo config
[related reddit post for context](https://www.reddit.com/r/Gentoo/comments/12q7g5a/stuck_on_itit_ramdisk/)

text of Reddit post:
[SOLVED], see update on bottom.

Decided to go off the deep end and do an install with btrfs in luks.  Way out of my depth, but I cobbled pieces together from multiple sources, and the handbook. I know the right  way to do it would have been to do a basic install, or an install of an easier distro.  I used genkernel because I'm not advanced enough to configure it myself.  I ended up getting stuck on `loading init ramdisk` when I try to reboot into the new system.  Followed advice to add framebuffer support.  Am about to try adding 'nomodeconfig' `GRUB_CMDLINE_LINUX'. If this doesn't work I'm going to try with the distribution kernel.  If that doesn't work might give up on LUKS and btrfs, or maybe go back to Arch. or open SUSE.  I have a lot of respect for the Gentoo community, in addition to being impressive, you've been helpful when I posted here, no 'RTFM' energy detected, even though I am trying to read the FM


Update: solved one problem, have another: I made a lot of configuration changes, and I'm not sure which one solved the problem (this was time consuming enough without rebooting and chrooting for each change) and I'm not going to go and undo each config change at this moment to identify which one actually solved the problem. But if I figure out 'oh I changed this back' and it still works I'l update this post further.

- I read somewhere [lost the link, it was an issue in the Arch Linux github] that there were bugs in bios on thinkpads.  I flashed a usb drive to update the bios using [this article](https://www.cyberciti.biz/faq/update-lenovo-bios-from-linux-usb-stick-pen/) and went through the bios update process.

- I made sure that my graphics cards were configured: for the legacy nvidia driver that I am using had to mask the latest versions, unmask the legacy one.  I made sure I also configured for my built-in intel and had `VIDEO_CARDS="intel i965 nvidia"` in my `/etc/portage/make.conf`

- There are several questions on gentoo forums regading the `initializing ramdisk` hang, and almost all of them point to changing framebuffer kernel parameters.  So I tried to take their advice as much as possible.
- The [nvidia page](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers#Kernel_compatibility) for the Gentoo wiki says you're good to go with `genkernel all` but later gives a warning that says `Important The "Mark VGA/VBE/EFI....`. I checked the config for genkernel all generated using the `genkernel.conf`), and those choices are not enabled by default.  So I tried setting them but `DRM_SIMPLEDRM` (called `FB_DRM` on the wiki... they are in the samme place in genkernel so I think they are the same thing with different names) makes `FB_SIMPLE`, recommended on the forums, unvavailable.  So in the end I went with `CONFIG_FB_SIMPLEFB` (the other parameter parameter from the wiki) and `FB_SIMPLE` along with `FB_VESA` and `FB_EFI`.

but keep in mind that all my tweaks might not be doing everything; it might have just been the BIOS the whole time.


Because I don't remember exactly what I did, I made a [git repo](https://github.com/jam1015/gentoo_config) with my backed up gentoo config files.  But don't rely on them too much: now after decrypting I'm getting the error `give root password for maintenance or dype ctrl-d to continue` which I can't escape from, which means that I've actually messed up the installation. But someone can use them as a resource to solve this particular problem. Also no guarantee that these will be kept up to date with what I actually use.
