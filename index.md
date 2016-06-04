---
layout: post
title: How I build EA4 RPMs
---

# Building packages for EasyApache 4 with Open Build Service
This will house documentation on EasyApache 4 and how I build packages using it.

### These are a work in progress, and are my notes from building the EA4 Symlink Protection Patch packages.
* Make an OBS account at: https://build.opensuse.org/
* Create a new subproject under your Home Project.
* Add the EA4 repository with 'isn:cpanel:EA4'
* Add package to OBS
* Add repo file to build server: 'wget http://download.opensuse.org/repositories/openSUSE:/Tools/CentOS_6/openSUSE:Tools.repo'
* yum install osc build
* git clone https://github.com/JPerkster/ea-apache2.git
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
