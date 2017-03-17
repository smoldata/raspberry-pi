# Caring for your Raspberry Pi

Your Raspberry Pi requires that you be a little more careful with it than other computers. For one thing, all of its electronic components are out in the open, vulnerable to static shocks. Please get a [case](https://www.adafruit.com/products/2258) for your Pi if you don't have one already.

## Be careful with powering it down

Try to always shut down your Raspberry Pi the proper way, instead of just yanking its power. Here's how:

```
ssh pi@[ip address]
sudo shutdown -h now
```

Then give it 30 seconds to finish the shutdown process before you remove the power.

You may run into a situation where you can't access your Pi's command line, so in those cases you may not have any other choice. It's possible that losing power unexpectedly will corrupt the disk on your Pi, making it unable to boot back up. If that happens, don't worry, you can always re-flash the MicroSD card.

## Backup your disk image

If you end up with a corrupted disk on your Pi, it will help to have a recent backup of the system disk. I have a dedicated thumb drive for this, formatted as "OS X Extended" (because I use a Mac).

Power off your Raspberry Pi, then insert the MicroSD card in your computer. You will want to use the `dd` utility to copy the contents of the MicroSD card to an image on the thumb drive. Here's an example of how I backed up the `/dev/disk4` (which will probably be different on your computer) to `/Volumes/smoldata/backups` (which will *also* be different on your computer):

```
diskutil unmountDisk /dev/disk4
sudo dd bs=4m if=/rdev/disk4 | gzip > /Volumes/smoldata/backups/2017-02-25-smoldata.img.gz
sudo diskutil eject /dev/rdisk4
```

## Practice restoring from backup

It's good to get into the habit of backing up your Raspberry Pi MicroSD card, but _just as important_ is that you know how to restore from the backup. Think of it like a fire drill, you feel more safe in a building knowing how to get out in a hurry if you need to.

You don't need to verify each and every backup you make, it's mainly just a reassurance for yourself that your backups will actually do what you think they will do.

Here's how you would take that earlier backup and flash it back onto the MicroSD card. And again, the specific paths will be different on your machine.

```
gunzip /Volumes/smoldata/backups/2017-02-25-smoldata.img.gz
sudo dd bs=1m if=/Volumes/smoldata/backups/2017-02-25-smoldata.img of=/dev/rdisk4
```

Note that when restoring from an "expanded disk" .img file, it will take significantly longer than with the Raspbian system images you may have installed before.
