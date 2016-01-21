
### Bless your EFI

needed to create an Ubuntu USB key

article says i need to install rEFInd.  i hope this doesn't fuck up my bootloader.  I go to sourceforge to download, then get angry because sourcefource is HTTP only....

looking at the Gentoo on MacbookPro article, it says rEFInd must be loaded from OSX, which was confusing at first. however, i should still be able to build the image from Ubuntu, then load it in with `bless` on OSX

> NOTE: this is not true: rEFInd does not need to be loaded with `bless` on OSX (AFAIK).  
> I was able to load it just fine using `efibootmgr` under linux with root.

relieved to see that i can download rEFInd source via git.  bummed out to realize that i need to install from source to get it securely.

### building tianocore (overview)

building rEFInd has a few dependencies, one of which cannot be downloaded via a package manager (edk2/udk2014).  so i venture out to sourceforge again to see that the EDK2 project is only available via subversion ... ugh.  

so search around to find out whether i can download it securely over subversion.  i find that i can indeed with the svn+ssh:// protocol, but i cant get access to the server.  realize that i need to create a subversion account and set up an ssh key, like with github.  ok, so i do that, then i figure out how to check out the UDK2014.SP1 branch I want.  

so, the instructions at [tianocore](https://github.com/tianocore/tianocore.github.io/wiki/UDK2014_How-to-Build) state the following directions:

1) download the source (i checked it out from subversion, so it's different for me) and move it to a workspace $WKSP. I built in /usr/local/UDK2014.
2) Generate the openssl (require's building another project) 
  - dear god, the build stack is now like 7 levels deep!
  - Gentoo => rEFInd => UDK2014 => OpenSSL => Patched OpenSSL
3) Build the BaseTools in $WKSP/BaseTools
4) Finally, build tianocore UDK2014

### patching openssl 1.0.2d for tianocore

```shell
git clone git@github.com:openssl/openssl
cd openssl
git checkout OpenSSL_1_0_2d
```

realized that this is a vulnerable version of openssl, so i should probably figure out how to install a newer version of rEFInd or something.  on second thought, tianocore/UDK2014 shouldn't really use openssl for any networking... networking should absolutely not be enabled in rEFInd *except* for iPXE and network boots.  OpenSSL is used for hashing rEFInd and secure boot, AFAIK.  so the vulnerabilities discovered in OpenSSL do not apply here.

```shell
# cp OpenSSL checkout to $WKSP/

cd $WKSP
cd Crypto/.../openssl
patch -fdsa ../EFI.patch

# ran into error where patch couldn't locate this file
# needed to manually apply part of the patch to correct the filename openssl.conf.h.in
# then i needed to update install.sh and manually correct the reference to openssl.conf.h.in

# then ran install.sh and it copied the files to $WKSP/Crypto/include
```

### building tainocore

then i changed some configuration options, as described in the rEFInd BUILDING.txt

ultimately, i built tianocore, but ran into some really wierd problems


###

TODO: describe process of securing rEFInd
TODO: describe setting up secure boot on MBP?
TODO: describe encrypting drives and setting up LUKS
