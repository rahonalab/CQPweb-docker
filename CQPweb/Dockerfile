##############################################################################
# Dockerfile for CQPweb
#
# Use a multi-stage build:
#  - download and build cwb
#  - install CQPweb dependencies incl. cwb
##############################################################################


##############################################################################
### Build cwb
###
FROM debian:stretch-slim
LABEL maintainer="egon w. stemle <egon.stemle@eurac.edu>"
LABEL maintainer="Luigi <luigi@rahona.be"
ENV LANG C.UTF-8
ARG CWB_REVISION=HEAD

# Set APT::Get::Assume-Yes
RUN echo 'APT { GET { Assume-Yes "true"; }; };' >> /etc/apt/apt.conf
RUN echo 'APT { GET { Fix-Broken "true"; }; };' >> /etc/apt/apt.conf

# Install system utils
RUN apt-get update \
    && apt-get install \
        subversion

# Get source
WORKDIR /usr/local/src
ADD http://svn.code.sf.net/p/cwb/code/cwb/trunk cwb.revision
#Cwb
RUN svn co -r ${CWB_REVISION} http://svn.code.sf.net/p/cwb/code/cwb/trunk/ cwb
RUN grep -E '^VERSION = ' cwb/definitions.mk | cut -c11- > cwb.version

###
#Compile cwb
# Installation steps from
# http://svn.code.sf.net/p/cwb/code/cwb/trunk/install-scripts/cwb-install-ubuntu
RUN apt-get update \
    && apt-get install \
        autoconf bison flex gcc make pkg-config libc6-dev libncurses5 libncurses5-dev libpcre3-dev libglib2.0-0 libglib2.0-dev libreadline-dev perl-modules-5.24 libhtml-parser-perl

WORKDIR /usr/local/src/cwb
ENV PLATFORM=linux-64
ENV SITE=binary-release
RUN make clean \
    && make depend \
    && make cl \
    && make utils \
    && make cqp \
    && make install \
    && make realclean

#Install dependencies for cwb
#Cwb-perl

#Compile cwb-perl
#Following http://svn.code.sf.net/p/cwb/code/perl/trunk/CWB-CL/README
WORKDIR /usr/local/src
RUN svn co http://svn.code.sf.net/p/cwb/code/perl/trunk/ cwb-perl
WORKDIR /usr/local/src/cwb-perl/CWB
RUN perl Makefile.PL \
    && make \
    && make test \
    && make install


# Install generic dependencies for web app
# 1) the ones that come with apt-get
RUN apt-get update \
    && apt-get install \
        apache2 \
        libapache2-mod-php7.0 \
        libxml-libxml-perl \
        php7.0-xml \
	less


# Tiny helper to start apache2 in foreground
COPY starter /usr/local/bin/


CMD ["starter"]
