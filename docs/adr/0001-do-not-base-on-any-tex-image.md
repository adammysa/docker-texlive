# Do not base on any tex image

## Context and Problem Statement

The Docker image can be made from scratch or base (`FROM`) on an existing one.
When choosing an existing one, which one should be taken?

## Decision Drivers

* Issue: https://github.com/scottkosty/install-tl-ubuntu is not updated anymore
* Self-maintaining an image (from scratch) is hard and one repeats the mistakes, others have done
* Patching an existing image (via PRs) might lead to rejections

## Considered Options

* Do not base on any tex image
* base on https://github.com/janweinschenker/docker-texlive/blob/master/Dockerfile - Ubuntu 17.10, texlive-full
* base on https://github.com/thomasWeise/docker-texlive/blob/master/image/Dockerfile - Ubuntu 16.04, small installation
* base on https://github.com/sumandoc/TeXLive-2017/blob/master/Dockerfile - Debian sid, install-tl-unx.tar.gz, LaTeXML
* base on https://github.com/rchurchley/docker-texlive/blob/latest/Dockerfile - Debian latest, texlive.iso; fetches config files via wget
* base on https://github.com/cdlm/docker-texlive - some more tools
* base on https://github.com/mtneug/texlive-docker/blob/master/Dockerfile - Ubuntu 17.04, texlive-full; nice badges; build.sh
* base on https://github.com/shuichiro-makigaki/docker-texlive-2017/blob/master/Dockerfile - Fedora latest
* base on https://hub.docker.com/r/ctarwater/docker-texlive/~/dockerfile/ - debian Jessie
* base on https://github.com/adinriv/docker-texlive/blob/master/Dockerfile - tlmgr -> based on docker-texlive-minimal - switched from install-tl-ubuntu to custom solution at https://github.com/adinriv/docker-texlive/commit/4c573e09bafff8da1ac121dd769a17bbbe0ca53b
* base on https://github.com/adinriv/docker-minimal-texlive/blob/master/Dockerfile - ubuntu:rolling, install-tl-unx, minimal setup
* base on https://github.com/harshjv/docker-texlive-2015/blob/master/Dockerfile - ubuntu 14.04
* base on https://github.com/chrisanthropic/docker-TeXlive/blob/master/Dockerfile - debian:jessie, texlive 2015, ISO
* base on https://github.com/camilstaps/docker-texlive/blob/master/Dockerfile - debian:jessie, config file in repo; `latexmk` does not work there. https://github.com/camilstaps/docker-texlive/issues/1
* base on https://github.com/dc-uba/docker-alpine-texlive - [alpine linux](https://hub.docker.com/_/alpine/), minimal texlive 2016

## Decision Outcome

Chosen option: "Do not base on any tex image", because [sumandoc/TeXLive-2017](https://github.com/sumandoc/TeXLive-2017) was the most promising, but it removed support for latexml at https://github.com/sumandoc/TeXLive-2017/commit/ef143d3d3396fdb243b7b7e1ff01f00a7eb26802 and https://github.com/sumandoc/TeXLive-2017/commit/6272e96f9b382ac3ba4b2338817fb80e5238ac5f.
The risk that other breaking changes come in cannot be taken.

We accept that

* We are based on debian sid, which constantly changes
* We will have to monitor the upstream repository if texlive 2018 is released and possibly adapt our Dockerfile.
* We get a large image - more than 4 GB.
* We have to install font packages separatetly for each font (e.g., [fonts-texgyre](https://packages.debian.org/sid/fonts/fonts-texgyre))

The only reasonable alternative seems to be <https://github.com/adinriv/docker-texlive>, which customizes the latex packages to be used.
