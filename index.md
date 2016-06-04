---
layout: post
title: How I build EA4 RPMs
---

# Building packages for EasyApache 4 with Open Build Service
This will house documentation on EasyApache 4 and how I build packages using it.

### These are a work in progress, and are my notes from building the EA4 Symlink Protection Patch packages.
* Make an OBS account at: https://build.opensuse.org/
* Create a new subproject under your Home Project.
  * In my example, I named it 'symlink_patch'
* Add two repositories to your project that link to CentOS 6 and CentOS 7 from the EA4 repository at 'isv:cpanel:EA4'
  * This ensures that any dependencies your packages need from EA4, it will be able to find them.
  * Any packages required from CentOS upstream are already set in EA4, and are inherited to your project from 'isv:cpanel:EA4'
* Add package metadata to OBS
  * In my example of building the symlink_patch packages, I needed to fork two packaegs (ea-apache2 and ea-apr).
  * I created two packages in my OBS project named 'symlink_patch' named 'ea-apache2' and 'ea-apr'
* Install required RPM build tools to interact with rpmbuild and OBS
  * I'm running on a CentOS 6 machine, so I used:
   * ```wget http://download.opensuse.org/repositories/openSUSE:/Tools/CentOS_6/openSUSE:Tools.repo```
   * ```yum install osc build```
* Fork cPanelIncs package into your own GitHub account, and then clone them onto your machine.
  * NOTE: The branch you use *MUST* be named the same as the Subproject you created in OBS. In my example, the project is named 'symlink_patch' so my git branches *must* be named 'symlink_patch'
  * ```git clone https://github.com/JPerkster/ea-apache2.git```
  * ```git checkout -b symlink-patch```
* Adjust Makefile to suit your needs
  * cPanel uses an internal Makefile that's not available publicly. It's best to revert the Makefile to the previous SHA
    * ```git revert $sha```
  * After the reversion, adjust the Makefile in a few places:
    * ```+OBS_PROJECT := Jperkster```
    * ```+OBS_USERNAME := $(shell grep -A5 '[https://api.opensuse.org]' ~/.oscrc | awk -F= '/user=/ {print $$2}')```
    * ```+BUILD_TARGET := home$(OBS_USERNAME):$(OBS_PROJECT):$(GIT_BRANCH)```
  * The reverted Makefile in most repositories provide a really cool feature, in that if you are building 1 project, but with multiple repositories, simply ensure that the git branch you are working on is named the same. 
    * Since we used the same git branch name as our OBS project name, all of our packages are built into 1 single yum repository. Neat!
* Send initial builds to OBS to make sure everything is a-okay
  * ```make obs```
* Make any changes you require
  * I had to add a patch to the SOURCES directory, and I told the .spec file to add my patch during compilation
* I like to verify I didn't screw anything up first by building the package locally on my machine:
  * ```make local```
* If your packages build locally, send them to OBS.
  * ```make obs```
