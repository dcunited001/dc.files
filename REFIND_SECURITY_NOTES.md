## Bootloader and rEFInd Security

Note: this is now a blog post, available on
[te.xel.io](http://te.xel.io/posts/2016-01-21-bootloader-and-refind-security.html).

To properly implement rEFInd, this means YOU NEED TO KNOW WHAT
YOU'RE DOING, especially if your security requires a more than usual
concern.  Otherwise, you're completely exposing super-root access to
your laptop.

I cannot underscore the importance of these, if security is a top
priority.  If it is, you should really not be running rEFInd.  You
should have a single bootable OS, with a minimal bootloader and you
should be using Secure Boot.

> Plz Note: this list should not be considered exhaustive, nor
> should this post be considered authoritative.  I'm just learning
> about bootloaders myself.  But this is a good starting point.

I should also note that configuring all these correctly is a pain.
It will require entering two passwords before you get to your OS
password.

### General Password Security Notes

For many, using a password manager sounds overly complicated,
especially one that's not sync'd to the cloud.  However, I can assure
you, it's actually way simpler. Just trust me. Everyone needs a
variable degree of privacy, but it's important to know what
protections your process actually gives you.

> Don't let someone own all your bases at once.

In general: use distinct passwords for LUKS, disk-encryption, password
safes, OS users and OS Super Users.  This provides proper security
through compartmentalization.

Furthermore, compartmentalize your use of a password manager by
creating multiple files (with separate keys!) and secure access to
these files *physically*.  That is, separate your most important
passwords (GMail, etc) from the rest of your passwords.  If you have
passwords for work or whatever, don't store them all in one place.

Don't store these password files on your computer and definitely not
in the cloud.  Instead, store the most crucial ones on a
LUKS-encrypted USB drive.  Or use a YubiKey, properly configured.

### (1) Signing your EFI bootloader!

If your bootloader is not signed, it can be modified.  This means a
malicious program could make modifications to your kernel images, in
memory, before your OS fully loads.  For OSX, Linux or Windows, this
means that arbitrary code could be loaded at the OS level as you're
booting.  This can wreak all kinds of havoc and/or mute any
protections you might think you have.

### (2) Configuring Secure Boot!

This involves signing your bootloader and configuring UEFI to reject
bootloaders that don't hash out...

### (3) Restricting your UEFI boot devices!

Someone who attains root access in an install can alter/update the
UEFI boot order or trigger an alternate UEFI boot.  This means they
can boot from CD/USB or whatever.  So even if you do not have
vulnerabilities in your systems configured, they can attain access
through an external linux ISO.  If you've configured things correctly,
this should not provide them with any more options.

### (4) Limiting your EFI/eEFInd boot options!

There are several problematic defaults for rEFInd, but which are
needed for newbs.  For example, if you don't restrict rEFInd boot
options, rEFInd scans accessible disks for bootable images.  Any of
these images can be booted.  You can configure rEFInd to allow users
to alter arguments passed to bootable images (partition, luks options,
etc.)  This is a very helpful option if you're learning about this
stuff, but also another avenue for an adversary with physical access.
And rEFInd offers other tools like MOKS and other options which are
enabled by default.  After you've got your bootloader set and signed,
you need to disable this stuff.

### (5) Utilizing whole-disk encryption (LUKS)!

If you're not using LUKS whole-disk encryption, this means anyone who
has 10 minutes access to your laptop can access rEFInd.  Using LUKS
should prompt you for a password before you can boot into rEFInd.

### (6) Encrypting your individual partitions!

If your disks are not encrypted, they are full accessible to any
OS/program than can be booted from EFI.  This means if you have a
linux install that you're just messing with, someone could obtain
access to all your OSX data, if that partition is not encrypted.
Using disk encryption for individual partitions means that you should
be prompted for a password before you boot into an OS from rEFInd.
That's a second password, before you get to your OS password.

### (7) Consistent Partition Table

When working with an existing OSX installation, you can end up with
a hybrid partition table, which is confusing.  You may need to image
your partitions and reload them.

### (8) Retaining access to your Mac's Restore Partition!

If things go wrong, you're going to want access to the restore
partition, as it provides alot of tools for troubleshooting these
problems, especially with regard to OSX and Mac hardware.
It really sucks to not have this when you need it.

### (9) While not locking yourself out of rEFInd!

Retain a bootloader backup that can be used in case you hose
your.  If you can't boot into rEFInd, you're going to have to
get creative to restore it.  This means backing up your EFI
partition and getting access to change your UEFI boot order.

I think.  Haven't gone through this myself.  Like I said.  I'm not an
expert here.

### (10) Using distinct keys for LUKS, Encryption and Passwords

Your individual partitions should not be encrypted with the same key.
You can't access a password manager at boot time.  You should not
reuse your LUKS key or any of your disk encryption keys as passwords
anywhere else.  Disk encryption is not very secure, especially if
adversaries have physical access or can image sections of your disk
multiple times.

That's all I can think of for now. Leave a comment below if you'd
like to discuss.

