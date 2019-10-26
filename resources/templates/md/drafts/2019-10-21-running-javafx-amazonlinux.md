
Recently I was trying to build the docker image for running my JavaFX ([DynamoDB Manager]()) application. While it proves to be possible (although looks like you have to apply various techniques to make
it work on all platforms: Linux, Windows and MacOS) I was currently focused on Linux platform only.

As I wanted it to be distributed over Docker and publish the image into the [Docker Hub]() I didn't want the image to be based on the Oracle JDK 8 due to the licensing issues, but use OpenJDK instead.

As JavaFX has been removed from Oracle/OpenJDK starting from version 9 as the separate distribution, I decided the image to be based on the Amazon Corretto JDK (although I have not researched whether BellSoft/Liberica JDK or Azul/ZuluJDK or AdoptOpenJDK come with the JavaFX within it). Also there are solutions on installing the openjfx distribution on top of the OpenJDK 11, although this is something I have to definitely investigate later, when building the self-contained application distribution [issue#]().

Might as well try Liberica JDK as it is also available on Docker Hub and the base image could be Debian. But still feels that I have to prepare the image by installing the missing libraries as also in the case of Amazon Linux below.

===

## Install Amazon Corretto

```
sudo yum install https://d3pxv6yz143wms.cloudfront.net/8.232.09.1/java-1.8.0-amazon-corretto-devel-1.8.0_232.b09-1.x86_64.rpm
```

## Missing libraries on Amazon Linux to support running JavaFX application

Here is the list of the libs I had to install on Amazon Linux to get my JavaFX application up and running:

```
sudo yum install libX11.x86_64 
sudo yum install gtk2.x86_64 
sudo yum install mesa-libGL.x86_64
sudo yum install glx-utils.x86_64 
sudo yum install mesa-dri-drivers.x86_64 
sudo yum install libXtst.x86_64
# JavaFX could not found any fonts system, these worked, but any installed font should work
sudo yum install google-noto-fonts-common.noarch 
sudo yum install google-noto-sans-fonts.noarch 
sudo yum install google-noto-serif-fonts.noarch 
```

## Configure SSH client and server

If you are using `.ssh/config` file add `ForwardX11 yes` and `ForwardAgent yes` (necessary?) into it. If you are not using `.ssh/config` then pass `-X` into `ssh` command.

Do I need to run `xhost +` on local machine?

On the server side for the sshd modify `/etc/ssh/sshd_config` and uncomment following options:

```
X11Forwarding yes
X11DisplayOffset 10 # not sure this one is absolutely necessary
X11UseLocalhost no
```

And install `xauth`.

## Limitations of Amazon Corretto JavaFX

`hostServices` is not supported, and so have to find another alternative to open the browser. Could use `Desktop.browse()` although as have been [reported](https://stackoverflow.com/questions/18004150/desktop-api-is-not-supported-on-the-current-platform) it is not really portable solution.

