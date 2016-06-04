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
* Add the EA4 repository with 'isv:cpanel:EA4'
  * This ensures that any dependencies your packages need from EA4, it will be able to find them.
  * Any packages required from CentOS upstream are already set in EA4, and are inherited to your project from 'isv:cpanel:EA4'
* Add package to OBS
  * In my example of building the symlink_patch packages, I needed to fork two packaegs (ea-apache2 and ea-apr).
  * I created two packages in my OBS project named 'symlink_patch' named 'ea-apache2' and 'ea-apr'
* Install required RPM build tools to interact with rpmbuild and OBS
  * I'm running on a CentOS 6 machine, so I used:
```wget http://download.opensuse.org/repositories/openSUSE:/Tools/CentOS_6/openSUSE:Tools.repo```
```yum install osc build```
* Fork cPanelIncs package into your own GitHub account, and then clone them onto your machine
```git clone https://github.com/JPerkster/ea-apache2.git```
  * git checkout -b symlink-patch
  * Revert makefile change
* Adjust Makefile to suit your needs
  * +OBS_PROJECT := Jperkster
  * +OBS_USERNAME := $(shell grep -A5 '[https://api.opensuse.org]' ~/.oscrc | awk -F= '/user=/ {print $$2}')
  * +BUILD_TARGET := home$(OBS_USERNAME):$(OBS_PROJECT):$(GIT_BRANCH)
* Send initial builds to OBS to make sure everything is a-okay
* Make changes to git repo
* make local ; Watch for any problems
* make obs
