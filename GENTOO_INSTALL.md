## Installing Gentoo on Macbook Pro 11,1

### setting up

actually the docs are more than sufficient.  they're phenomenally great!

one thing that happened was: `mirrorselect` didn't automatically create `/etc/portage/repos.conf/gentoo.conf`, which was a bit confusing when i saw warnings in `emerge --info` and other commands.  this file is only needed when you want to override some defaults like the sync protocol and mirror.

### configuring portage

i added     and vim to global use flags, even though doing so is likely pointless ... then i ran into circular dependencies when running `emerge --update --deep --newuse @world`

if global use flags are updated, run `emerge --update --deep --newuse @world` to update the system.  then run portage's depclean with `emerge -p --depclean` to remove the conditional dependencies that were emerged on the "old" system but that have been obsoleted by the new USE flags.  this is noted as a potentially dangerous option in the [USE docs](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE). When depclean has finished, run `revdep-rebuild` to "rebuild the applications that are dynamically linked against shared objects provided by possibly removed packages." and ensure that you emerge `revdep-rebuild` first.	



### timezone/locale

thinking about going with Zulu time, but it's exactly the same as UTC.  i went with `echo "America/New_York" > /etc/timezone` instead.  then ran `emerge --config sys-libs/timezone-data` to reconfigure timezone stuffs.

for locales, i referred to this [doc](https://docs.moodle.org/dev/Table_of_locales) for timezone data. i wanted to ensure i had these character sets, keyboards, etc on my system, although I'm not sure that leaving these out would prevent me from having them.

- english(iso & utf8)
- japanese(utf8 & euc-jp)
- chinese(simplified/traditional)
- hindi(for devenagari)
- thai
- greek
- korean
- russian

### configuring init system

i went with systemd instead of gentoo since that's what's in CoreOS and so i referenced the docs for [systemd on gentoo](https://wiki.gentoo.org/wiki/Systemd).

### maintaining gentoo



### after chroot:

```bash
source /etc/profile
export PS1="[chroot] $PS1"
emerge-webrsync
emerge --sync --quiet
eselect profile list
eselect profile 5 # gnome/systemd
# add -march=native to make.conf
# add -j7 to make.conf
# setup USE flags
echo "America/New_York" > /etc/timezone
emerge --config sys-libs/timezone-data
# edit /etc/locale.gen to add locales
eselect locale list
eselect locale set 6 # us_EN.UTF-8
env-update && source /etc/profile
ln -sf /proc/self/mounts /etc/mtab

# update and build stuff 
# (takes forever!! run in background)
emerge --ask --update --dep --newuse @world
eselect --depclean

# misc hardware
emerge --ask sys-apps/pciutils
emerge --ask sys-kernel/linux-firmware

# wifi (for open-source b43 driver, which doesn't include 802.11n)
# echo "sys-firmware/b43-firmware" >> /etc/portage/package.accept_keywords
# echo "sys-firmware/b43-firmware Broadcom" >> /etc/portage/package.license
# emerge --ask sys-firmware/b43-firmware 

# wifi (prorietary broadcom-sta)
emerge --ask net-wireless/broadcom-sta

# for nvidia
emerge --ask --unmerge sys-kernel/genkernel
emerge --ask sys-kernel/genkernel-next

# for xorg

# for gnome


# for systemd


# for cuda
emerge --ask dev-util/nvidia-cuda-sdk
emerge --ask dev-util/nvidia-cuda-toolkit

```

for proprietary [nvidia drivers](https://wiki.gentoo.org/wiki/NVidia/nvidia-drivers), you must disable nouveau.

for pretty much everything, i tried to get as many kernel configs taken care of beforehand.  it may be better to setup gentoo, then the graphical user interface to keep any problems simpler.  

xorg should be completed prior to gnome. [xorg docs](https://wiki.gentoo.org/wiki/Xorg/Guide)

[gnome](https://wiki.gentoo.org/wiki/GNOME/Guide) installed and then [systemd](https://wiki.gentoo.org/wiki/Systemd).  guide to [installing both](https://wiki.gentoo.org/wiki/Systemd/Installing_Gnome3_from_scratch) after getting up and running

for [signed kernel modules](https://wiki.gentoo.org/wiki/Signed_kernel_module_support)

for bluetooth, refer to [wiki docs](https://wiki.gentoo.org/wiki/Bluetooth)

macbook pro config options:

- In Media USB Adapters:
  - missing USB Video Class
- In PCI Sound Devices:
  - Missing Intel HD Audio
- OBS requires pulseaudio
  - under HD-Audio, change preallocate setting to 2048
- pulseaudio requires alsa
  - under hd-audio, enable all options

- [ALSA](https://wiki.gentoo.org/wiki/ALSA) and [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) config

required for audio: add the following to /etc/modprobe.d/alsa: `options snd_hda_intel model=mbp101`
