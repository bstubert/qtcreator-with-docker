# Docker Builds from QtCreator

Imagine that you face the following situation. Your development PC runs Ubuntu 16.04.
One of your target embedded system runs Ubuntu 18.04. When you want to try out your
latest application changes on the target system, you must perform four tasks:

* You build the Qt application with Docker for the target system.
* You stop the application on the target system.
* You copy the application from the development PC to the target system with scp.
* You start the application on the target system.

As you have done on your PC thousands of times, you would love to hit Ctrl+R (Run) in
QtCreator and have QtCreator perform the four tasks automatically. I’ll show you how
in this post.

Note that the Docker container could contain any environment to build Qt applications.
For example, you could set up a Qt Yocto SDK in the container to cross-build Qt applications
for ARM SoCs. You can run your Qt application both on your PC and on your target system
by simply switching kits in QtCreator – while staying in the same QtCreator session.

See [my post](https://www.embeddeduse.com/2020/04/13/docker-builds-from-qtcreator/)
for step-by-step instructions.
