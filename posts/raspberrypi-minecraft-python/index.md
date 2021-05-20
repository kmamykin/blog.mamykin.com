---
draft: true
---

* Inspiration
  Something like this https://projects.raspberrypi.org/en/projects/getting-started-with-minecraft-pi/4 but
    a) Programming on a Mac with remote connection to a Pi running MC server
    b) Full featured MC server/Multi-player
    c) Expose to other kids to connect to for class programming lessons
* Raspberry Pi Setup
  * Use Raspbian (lite) for headless/keyboardless install https://www.raspberrypi.org/downloads/raspbian/

  * Copying an OS image to an SD card using Mac OS cli https://www.raspberrypi.org/documentation/installation/installing-images/mac.md
  * Solid guide, uses Spigot server, but need to install other plugins https://pimylifeup.com/raspberry-pi-minecraft-server/
* minecraft-pi?
* Spigot server
  Difference between Spigot, Bukkit, CraftBukkit
    https://bukkit.gamepedia.com/FAQ
    https://www.spigotmc.org/wiki/what-is-spigot-craftbukkit-bukkit-vanilla-forg/
  * Install CraftBukkit and Spigot with BuildTools https://www.spigotmc.org/wiki/buildtools/
  java -Xmx1024M -jar BuildTools.jar --rev 1.14.4
  copy spigot-1.14.4.jar to a separate empty folder (different location from where it was built by BuildTools.jar)
  java -Xms512M -Xmx1008M -jar /home/pi/minecraft/spigot-1.14.4.jar nogui


* Minecraft Pi https://www.minecraft.net/en-us/edition/pi/ https://minecraft.gamepedia.com/Pi_Edition
  * Stripped minecraft version + Java plugin + mcpi (original)
* Spigot plugins (RaspberryJuice)
  * RaspberryJuice https://github.com/zhuowei/RaspberryJuice
    download https://github.com/zhuowei/RaspberryJuice/raw/master/jars/raspberryjuice-1.12.1.jar


* mcpi package https://github.com/martinohanlon/mcpi
* "Learn to Program with Minecraft" book - Buy it!
  * https://nostarch.com/programwithminecraft
  * https://nostarch.com/minecrafthelp
  * Download (Minecraft_Tools_Mac.zip) contains spigot server + RaspberryJuice + mcpi (modded?)


Fun ideas with this setup (lessons):
  From https://projects.raspberrypi.org/en/projects/getting-started-with-minecraft-pi/4
    Dropping blocks as you walk
    Playing with TNT blocks
    Fun with flowing lava
  https://gist.github.com/noahcoad/fc9d3984a5d4d61648269c0a9477c622
  https://gist.githubusercontent.com/arrowtype/5ab1acc544671a9408fe285dfdc5fc1b/raw/5f38ce5f4826056bc5b9005a1b1c1aa94e819581/minecraft-cubes-and-towers.py


https://github.com/Macuyiko/minecraft-python
