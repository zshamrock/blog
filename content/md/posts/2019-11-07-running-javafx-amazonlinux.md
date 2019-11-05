{:title "Running JavaFX application on Amazon Linux remotely over SSH using Amazon Correto"
:layout :post
:tags ["javafx" "openjdk" "aws"]
}

Recently I was trying to build the docker image for running my JavaFX [DynamoDB Manager](https://github.com/zshamrock/dynoman) application. While it proves to be possible (although it looks like you have to apply various techniques to make it work on all platforms: Linux, Windows, and macOS) I was currently focused on the Linux platform only.

As I wanted it to be distributed over Docker and publish the image into the [Docker Hub](https://hub.docker.com/) I didn't want the image to be based on the Oracle JDK 8 due to the licensing issues but use OpenJDK instead.

As JavaFX has been removed from Oracle/OpenJDK starting from version 9 as the separate distribution, I decided the image to be based on the Amazon Corretto JDK (although I have not researched whether BellSoft/Liberica JDK or Azul/ZuluJDK or AdoptOpenJDK come with the JavaFX within it as directly from Docker Hub). There will be the separate post highlighting the state of JavaFX within various OpenJDK distributions.

Also, there are solutions to install the openjfx distribution on top of the OpenJDK 11, although this is something I have to definitely investigate later when building the self-contained application distribution.

Might as well try Liberica JDK as it is also available on Docker Hub and the base image could be Debian. But still feels that I have to prepare the image by installing the missing libraries as also in the case of Amazon Linux below.

<div class="alert alert-warning" role="alert">
  All the below commands assume you SSH into your AWS EC2 instance and run all the commands from there.
</div>

### Install Amazon Corretto

```
sudo yum install https://d3pxv6yz143wms.cloudfront.net/8.232.09.1/java-1.8.0-amazon-corretto-devel-1.8.0_232.b09-1.x86_64.rpm
```

### Install missing libraries on Amazon Linux to support running JavaFX application

Here is the list of the libs I had to install on Amazon Linux to get my JavaFX application up and running:

```
sudo yum install libX11.x86_64 
sudo yum install gtk2.x86_64 
sudo yum install mesa-libGL.x86_64
sudo yum install glx-utils.x86_64 
sudo yum install mesa-dri-drivers.x86_64 
sudo yum install libXtst.x86_64
# JavaFX could not found any system fonts, these worked, but any installed font should work
sudo yum install google-noto-fonts-common.noarch 
sudo yum install google-noto-sans-fonts.noarch 
sudo yum install google-noto-serif-fonts.noarch 
```

### Debug JavaFX Java system options

By setting the following Java system options `-Djdk.gtk.verbose=true -Djavafx.verbose=true`, for example, 
using [`JAVA_TOOL_OPTIONS`](https://stackoverflow.com/questions/28327620/difference-between-java-options-java-tool-options-and-java-opts) produces the following out statements 
which help to debug which libraries are missing/used:

```
Picked up JAVA_TOOL_OPTIONS: -Djdk.gtk.verbose=true -Djavafx.verbose=true
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libprism_es2.so from relative path
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libprism_sw.so from relative path
JavaFX: using com.sun.javafx.tk.quantum.QuantumToolkit
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libglass.so from relative path
checking GTK version 2
trying GTK library libgtk-x11-2.0.so.0
using GTK library version 2 set libgtk-x11-2.0.so.0
Glass GTK library to load is glassgtk2
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libglassgtk2.so from relative path
Launching application directly
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libjavafx_font.so from relative path
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libjavafx_font_freetype.so from relative path
Loaded /usr/lib/jvm/java-1.8.0-amazon-corretto/jre/lib/ext/../amd64/libdecora_sse.so from relative path
```

Class `com.sun.glass.ui.gtk.GtkApplication` (is also where `jdk.gtk.verbose` is used) is the good place to debug the issue with loading Gtk if the application fails to start. Interesting that this class is only available in the OpenJFX implementation, but not in the OracleJDK.

### Configure SSH client and server

If you are using `.ssh/config` file add `ForwardX11 yes` into it. If you are not using `.ssh/config` then pass `-X` into `ssh` command.

On the server-side:

- Modify `/etc/ssh/sshd_config` and uncomment following options:


```
X11Forwarding yes
X11DisplayOffset 10 # optional
X11UseLocalhost no
```

- And install `xauth`.

Here is the example of the running application:

<img src ="/img/remote-ssh-dynoman.png" alt="Remote SSH Dynoman" class="img-thumbnail">

### Limitations of Amazon Corretto JavaFX

`hostServices` and in general webkit and media API are [not supported](https://github.com/corretto/corretto-8/issues/26), and so have to find another alternative to open the browser. Could use `Desktop.browse()` although as have been [reported](https://stackoverflow.com/questions/18004150/desktop-api-is-not-supported-on-the-current-platform) it is not really a portable solution.

- - -

Overall it took some time to figure out the missing libraries, and go through trials and errors to make it up and running, so next step would be to build the corresponding Docker image and publish it on Docker Hub (although it will be likely based on another OpenJDK distribution rather than Amazon Correto).
