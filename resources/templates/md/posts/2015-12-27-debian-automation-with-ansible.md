{:title "Debian automation with Ansible"
:layout :post
:tags ["practice" "devops" "craft"]
}

This year (2015) I have to re-install/partially re-install Debian on my laptop several times. Not because I have a lot of free time or like to do it :) 

First time I've upgraded into SSD from HDD, which then has been broken in 6 months, so I have to switch back to old HDD, but as I had Debian 6 (squeeze) installed there I've upgraded to wheezy and then to jessie. Which went fine. 

Due to historical reasons (as Debian on my laptop, when first installed, was 32 bit version, and since then successfully (even from university times) lived on the laptop) I was running a 32 bit version of Debian. But more and more tools nowadays either don't even have a 32 bit version, or simply don't work well on 32 bit platform, Docker was mainly one of the main motivation (not single although) switching to 64 bit, and it means I have to install a new latest Debian version from scratch again. So, I [diditagain](https://github.com/zshamrock/ididitagain) a third time, bought a new SSD and installed latest Debian 8 (jessie) on it.

I guess, everybody while using any device, be it a laptop, phone, tablet, reader, literally anything, sooner or later starts to care not about the device itself, but about content, settings, setup on the device, which then become a valuable asset by its own. And while I don't need much to work productivily on my laptop, there are bits which I care about - like various dot files, fonts settings, colors, specific set of tools installed and configured, shortcuts, and many others, doing the same for the third time, as I've wanted to automate my working environment for a long time, it was a right moment to do so finally.

<img src="https://raw.githubusercontent.com/zshamrock/ididitagain/master/demo/demo.gif" alt="Demo" class="img-thumbnail">

### Value of automation

We as developers, following a good design principles, trying to eliminate code duplications whenever possible. Especially if we have to do the same things more than 2 times. 

In the past years this also became true and a standard for the production/environment as well, with rise of different kind of provisioning tools like Chef, Puppet or Ansible, VM managers, like Vagrant, and containerization, like Docker.

Nobody would disagree, that having a way to build a reproducible environment when required is a good thing. When nowadays almost everything is run in cloud and instantiate a new VM (be it an AWS, DigitalOcean, or many other cloud providers), be able to provision and have the exact setup you can trust in a minute is a great thing.

So, this is exactly what I am doing, but for my working environment on my laptop. So, if ever I have to switch to another laptop, SSD, setup Linux at work, or on one of my family or friend's laptop, applying the same setup literally in less than one minute (excluding the time required to download the necessary dependencies or archives), I can't express enough how valuable and time saving it is.

### Why Ansible?

While there are different types of similar tools which could do the job, I went with Ansible. The main reason is simplicity, and an extra benefit of being agent-less.
I had an experience working with Puppet, and at work, although unfortunately, we have a dedicated devops team, although things seem to be [improved](posts/2015-09-27-you-are-the-devops.html) later, we use Chef, I personally found Ansible the simplest and very human readable, yet powerful to let me do whatever I had in mind.

### Show me the code

Well, <i class="fa fa-github-square"></i> [ididitagain](https://github.com/zshamrock/ididitagain). 

[README.md](https://github.com/zshamrock/ididitagain/blob/master/README.md) will be the best place to start with, and explore then code in [roles](https://github.com/zshamrock/ididitagain/tree/master/roles) directory of the project.

### Reasonable granularity

It is possible to run either a whole group of related tasks like `dev`, `system` or `media`, or just a specific one, like `clojure`, `hipchat`, `jdk`, or `idea`. Or multiple of them separated by comma.

Ansible also gives a nice way to list all the available tags: 

```
$ ansible-playbook -i hosts --list-tags site.yml 

playbook: site.yml

  play #1 (localhost):  TAGS: []
      TASK TAGS: [alternatives, apt, atom, chrome, clojure, dev, docker, dot-files, dropbox, git, gnome, go, hipchat, idea, jdk, maven, media, packages, permissions, productivity, screencast, sdkman, skype, slack, spotify, system, tools, viber, vim]

```

### Is it not time consuming?

It depends on how you consider it. Yes, it took some time and energy to build the project into state it is currently. But, apart from giving a confidence and peace of mind, that all my changes and setup are in one place, stored on Github, and I can easily track all the changes are happening into my system, it actually helps to avoid spending (waste?) time tweaking OS and tools, as right now, if I want to keep all the changes tracked, I have to do more work, which unless I am really annoyed by the current settings or behaviour I do not do.

Also I do not automate everything. If I need one time action, or something I have to do now, but do not want it to be part of the system in the future, I just do it manually.

### Iterative approach

If you track [commits](https://github.com/zshamrock/ididitagain/commits/master) changes, you will see that the original version, which although has worked, is slightly different from its current state. And this always amazes me - the evolution of software (and this is true for any creative work, like art or writing a book, or this blog post as well, by starting small and simple, and grow organically). 

So, if you check the changes, you will see that originally I used individual sub-directories in one `local` role, but tags "inheritance" didn't work as I've expected ([https://github.com/ansible/ansible/issues/13674](https://github.com/ansible/ansible/issues/13674)), so I have to switch to multiple roles, which I think is even better, as it allows, to not only separate `tasks/`, but `files/`, `templates/` and `handlers/` as well, and it is clear which files or templates belong to which role comparing to having all together in one place, which was a good starting point, but doesn't scale (in terms of ease of maintenance, and keeping code simple) well when more and more tasks are developed.

### Can I use it too?

Absolutely. It doesn't have any personal details in it, except `me` variable in `group_vars/all`, which you might want to change. 

Yes, the tools and setup are the ones I got used to and found useful, but all of them are very well chosen and crafted, so I believe they will satisfy almost everybody (unless you are Emacs guy :), as a nice set of sensible defaults. 

It is specifically will be beneficial for you if you are Java, Clojure or Go developer (or all of them depends on the task at hand), using Vim as a main text editor, apart from IntelliJ IDEA.

The beauty is that you don't need to have all or nothing. You can just use part of it using `--tags` to manage, lets say Clojure or JDK or even IntelliJ IDEA installation, or anything else the project has you need.


### All my private and professional life in one project

It is true. By running all the tasks the project has you get the exact same setup I have on my laptop, which might give you a sense of the tools, setup and maybe even the workflow I use daily.

### Simplicity, automation and visibility

As I say in [About](pages/about.html) simplicity, automation, and visibility are a few of my core values, and this project is a nice example of being able to automate my working environment while using a simple tool like Ansible, as well keeping the code simple and clean, and giving a visibility of all the changes done in the system over time.

Very nice!
