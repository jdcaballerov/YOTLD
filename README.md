The Year of the Linux Desktop
From Macbook to Linux
=======

After many years of using Macbook Pros, I’m finally saying “Enough!”. I have a great 2010-2012 Macbook Pro that still rocks. But in 2014 I decided to buy a mid-2014 model and after two years it started falling apart. First, one of the speakers started sounding tinny. Then the power plug port pins burnt out. Then, just after the warranty expired, came [stain gate](https://en.wikipedia.org/wiki/Staingate): the anti-reflective coating started to peel off, staining large portions of the screen. I dug around on the internets for advice and solved the problem by rubbing the film off the glass with metal polish. A few months later, the other speaker went the way of the first, and the power plug port pins burnt out again. Finally, the screen started to flicker (and it wasn't the LVDS display cable). Enough!!

<img src="https://i0.wp.com/lopezdoriga.com/wp-content/uploads/2015/07/Staingate-Apple-Staingate2.jpg?resize=883%2C481&ssl=1" alt="Apple staingate"/>

I decided to go back to Linux on a Lenovo and tweak it to make it as similar as possible to what I was used to on macOS. Here’s my guide for improving the (Gnome) Linux desktop experience—2020 really is the year of the Linux desktop!


## Distro

Most major distros will do the job, however some of them come with useful out-of-the-box features. Two are especially worth mentioning:

1. [Pop OS](https://pop.system76.com/)
    This distro not only stands out for not being loaded with crapware (unnecessary, probably useless software), it also has its own distro version to manage Nvidia [video cards](https://en.wikipedia.org/wiki/Video_card). Nvidia cards are usually a headache for Linux users since some cards only work with signed proprietary drivers. And once you manage to get the card working, your pain isn’t over: you find out you've given birth to a power hog and your battery won't last. The latest Pop OS allows you to just turn off the GPU [without manually blacklisting modules](https://www.reddit.com/r/pop_os/comments/cu4au4/fix_battery_life_with_nvidia_gpu_mx250_on_lenovo/) and use the onboard card or operate in hybrid mode. There’s no need for [ACPI table hacks](https://major.io/2020/01/24/disable-nvidia-gpu-thinkpad-t490/) or [vgaswitcheroo](https://01.org/linuxgraphics/gfx-docs/drm/gpu/vga-switcheroo.html). The ability to turn off the GPU is integrated into the desktop. One click, finito. It also includes a simple tiling manager that can be turned on and off at will with a single click.
    
    
2. [Fedora](https://getfedora.org/en/workstation/download/)
    Fedora does a [great job](https://fedoraproject.org/wiki/Changes/ImprovedLaptopBatteryLife) with [power consumption](https://www.youtube.com/watch?v=mypteFGjwH4) and the latest version (32) works well out-of-the-box. With Fedora, many new features, such as [Wayland](https://wayland.freedesktop.org/), become available sooner than with other major distros. The troubles with docker are mostly [gone](https://fedoramagazine.org/docker-and-fedora-32/).


## Battery life

Battery life has always been a nightmare with Linux laptops. Fortunately, there are distros like Fedora that have come with reasonable defaults so that laptops are more power efficient out-of-the-box. The old tools have evolved a lot. I am talking about the duo [TLP](https://linrunner.de/tlp/) and [powertop](https://01.org/powertop/).

To optimize battery power, the first thing to do is to establish a baseline while running on battery power. I prefer to start measuring idle power consumption (discharge rate depends on the workload). If Windows is available, [BatteryInfoView](https://www.nirsoft.net/utils/battery_information_view.html) is useful for setting a goal by looking at the charge/discharge rate value.

1. powertop: `sudo powertop -c` will calibrate the measurement and present an estimate.
2. [powerstat](https://github.com/ColinIanKing/powerstat): `sudo powerstat` will wait for a delay and take measurements over time to generate statistics.

If battery life is a major concern, it is advisable to be able to completely turn off the GPU (Graphic Processing Unit). The former section on Pop OS contains some links and strategies for the task. For Nvidia GPUs, the command `ndvia-smi` should confirm the card is turned off. After turning off the GPU, set a new idle power consumption baseline.

[TLP and powertop can get along together](https://gvisoc.com/tech/linux/2020/04/26/Lenovo-ThinkPad-T490s-a-Battery-Review-under-Linux.html), however, it's better if only one of them is in charge. I usually start with TLP, check and modify settings using `sudo tlpui` and then check with `sudo powertop` to see if there’s any optimization that’s not turned on. With this new tweak I can establish a new baseline.

When you feel comfortable, you can start optimizing on your usual work load (browser + code editor + bluetooth headset, etc). Powertop is handy here.


Some power hogs to watch out for:

- Docker, containerd (switch them to on demand)
- Browser tabs with misconfigured ads, mining, flash, etc.
- Social networks (they are constantly polling or receiving socket events)
- Unused interfaces (use `sudo ifdown interface_name down` to turn them off)
- Any disk indexer
- Monitoring software
- Daemons checking for updates, file modifications, and syncs

To get some extra milliwatts, you can try correcting throttling (if necessary) and undervolting. If undervolting is used, it’s advisable to test it on your usual workload and try to suspend and unsuspend the laptop to test for stability. It’s typical to undervolt identically on the CPU and Cache.

- https://github.com/erpalma/throttled
- https://github.com/kitsunyan/intel-undervolt
- https://github.com/georgewhewell/undervolt

Battery lifespan

For some Lenovo laptops, TLP allows you to set a different charge/discharge threshold so that the battery won't charge while it operates in the threshold band. This will prolong the battery life span.

## Scaling 

Screen scaling out-of-the-box on Linux usually sucks since it tends to be tiny. To correct the scaling, a common solution for Gnome users is to install the tweaks package:

```bash
sudo apt install gnome-tweaks
gnome-tweaks
```

and then go to the Fonts panel and adjust the scaling factor between 1.4 and 1.5 for a similar scaling to a Macbook Pro.

## Mouse

After switching from a Macbook Pro, your new trackpad can feel awful. You might need to investigate further, but I found that after using it for a few days, I got comfortable with this adaptive profile of Gnome tweaks.

1. Open `gnome-tweaks`
2. Keyboard & mouse panel
3. Acceleration profile: choose adaptive
4. Check disable while typing
5. Check mouse click emulation: fingers

## CTRL Key

Most of the shortcuts are the same on Linux, but instead of the command key you use the control key. I prefer macOS shortcuts so I remapped the keys using `gnome-tweaks`.

1. Go to Keyboard & Mouse Panel
2. Click on Additional Layout Options
3. Find Alt/Win Key Behavior
4. Check: CTRL is mapped to Alt; Alt is mapped to Win.

A full working solution is to use [kinto](https://github.com/rbreaves/kinto)

If more options are needed, you might need to use `xmodmap` or `kinto`

http://xahlee.info/linux/linux_xmodmap_tutorial.html
https://github.com/rbreaves/kinto

## Shortcuts 

If you don't use [kinto](https://github.com/rbreaves/kinto), you can set up manually.

After many years of using Macbooks, I’m used to Apple shortcuts. In Gnome-based systems you can manually configure or import [using this script](https://gist.github.com/jdcaballerov/2fcfb817e332e290bf0943d57b8b4bfc) from [askubuntu](https://askubuntu.com/questions/26056/where-are-gnome-keyboard-shortcuts-stored).

to export key bindings

`./keybindings.pl -e /tmp/keys.csv`

to import 

`./keybindings.pl -i /tmp/keys.csv`

[Here you can find my bindings](https://gist.github.com/jdcaballerov/9d08ff82b701cd5b54c70f1a9f651ef7)

Since alt was remapped, window related shortcuts should bind to control. I use some other shortcuts included with my bindings to try to match my [spectacle](https://www.spectacleapp.com/) profile on macOS.

## Multitouch Gestures

You can use Fusuma to enable your Linux to recognize swipes or pinchs and assign commands.
https://github.com/iberianpig/fusuma


## Gifs and screenshots  (peek and flameshot)


### Screencast

```bash
sudo apt-get install byzanz
sudo apt-get install appmenu-gtk2-module appmenu-gtk3-module

git clone https://github.com/lolilolicon/xrectsel.git
sudo apt-install autoconf
./bootstrap

```

```bash
#!/usr/bin/env sh

[ $# -lt 2 ] && \
  echo 'You have to pass a duration in seconds and a filename: "gif.sh 10 /tmp/record.gif"' && \
  exit 1

byzanz-record                                        \
  --cursor                                           \
  --verbose                                          \
  --delay=2                                          \
  --duration="${1}"                                    \
  "$(xrectsel "--x=%x --y=%y --width=%w --height=%h")" \
  "${2}"
```
from: https://github.com/lupoDharkael/flameshot/issues/172#issuecomment-466657937

## Image and video previews

```
sudo apt install gnome-sushi
```


## Emoji Keyboard

The emoji keyboard shortcut can collide with some text editors as vscode—since it is configured as `shift+ctrl+e` - so it needs to be changed. To change it, open `ibus-setup` and remap it so that it won't collide.

A gnome shell extension exists https://extensions.gnome.org/extension/1162/emoji-selector/

## Clipboard Manager

For a very simple clipboard manager, I use clipit. 

`sudo apt install clipit`

Another option is installing a gnome-shell extension
https://extensions.gnome.org/extension/779/clipboard-indicator/
and then syncronize clipboards with `autocutsel` 

```bash
sudo apt install autocutsel -y
# add to .bashrc
autocutsel &
autocutsel -s PRIMARY &

```

## Password Manager

[KeepassXC](https://keepassxc.org/download/#linux) can import 1passwd files and many others.


## xournal

To annotate and sign PDFs, Apple's preview works amazingly well. On Linux [xournal](https://github.com/xournalpp/xournalpp)
can accomplish pretty much the same thing.

---

As Apple continues to downgrade the quality of its hardware (to match its pricey, disposable adaptors), we need to look for other options. These steps will give you a much better user experience—comparable to that of macOS—on a Gnome-based desktop on Linux. 

For anyone who wants to contribute more tips and tweaks, you can send your contributions to [this Github repository](https://github.com/jdcaballerov/YOTLD/). 



