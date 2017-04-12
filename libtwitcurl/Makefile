# Hey Emacs, this is a -*- makefile -*-
# The twitcurl library.. a Makefile for OpenWRT
# Makefile-fu by John Boiles
# 28 September 2009

LIBNAME = twitcurl
SRC = $(LIBNAME).cpp
STAGING_DIR = 
INCLUDE_DIR = $(STAGING_DIR)/usr/include
LINCLUDE_DIR = $(STAGING_DIR)/usr/local/include
LIBRARY_DIR = $(STAGING_DIR)/usr/lib
LLIBRARY_DIR = $(STAGING_DIR)/usr/local/lib

CXX = g++
CC = gcc
REMOVE = rm -f
COPY = cp
REMOTEIP = 192.168.1.30

# Compiler flag to set the C Standard level.
# c89   - "ANSI" C
# gnu89 - c89 plus GCC extensions
# c99   - ISO C99 standard (not yet fully implemented)
# gnu99 - c99 plus GCC extensions
# CSTANDARD = -std=gnu99

# Place -D or -U options here
CDEFS =

# Place -I options here
CINCS = 

CFLAGS =$(CDEFS) $(CINCS) $(CSTANDARD)
CXXFLAGS = -std=c++14

OS := $(shell uname)
SONAME =

ifeq ($(OS),Darwin)
	SONAME = -install_name
	LDFLAGS += -Wl,-rpath,$(STAGING_DIR)/usr/lib
else
	SONAME = -soname
	LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib
endif

all: target

target: $(SRC) $(LIBNAME).h
	$(CXX) -Wall -fPIC -c -I$(INCLUDE_DIR) $(SRC) oauthlib.cpp urlencode.cpp base64.cpp HMAC_SHA1.cpp SHA1.cpp
	$(CXX) -shared -Wl,$(SONAME),lib$(LIBNAME).so.1 $(LDFLAGS) -o lib$(LIBNAME).so.1.0 *.o -L$(LIBRARY_DIR) -lcurl
#clean project.
clean:
ifeq ($(OS),Darwin)
	$(REMOVE) lib$(LIBNAME)*.so.1.0
	$(REMOVE) $(LIBNAME).o
	$(REMOVE) *.o
else
	$(REMOVE) lib$(LIBNAME)*.so.1.0
	$(REMOVE) $(LIBNAME).o
	$(REMOVE) *.o
	$(REMOVE) $(LIBRARY_DIR)/lib$(LIBNAME).so*
endif

# Install library to local openwrt library directory
install: all
ifeq ($(OS),Darwin)
		$(COPY) lib$(LIBNAME).so.1.0 $(LLIBRARY_DIR)
		$(COPY) $(LIBNAME).h $(LINCLUDE_DIR)/
		$(COPY) oauthlib.h $(LINCLUDE_DIR)/
		ln -sf $(LLIBRARY_DIR)/lib$(LIBNAME).so.1.0 $(LLIBRARY_DIR)/lib$(LIBNAME).so
		ln -sf $(LLIBRARY_DIR)/lib$(LIBNAME).so.1.0 $(LLIBRARY_DIR)/lib$(LIBNAME).so.1
else
	install: all
		$(COPY) lib$(LIBNAME).so.1.0 $(LIBRARY_DIR)
		$(COPY) lib$(LIBNAME).so.1.0 $(LLIBRARY_DIR)
		$(COPY) $(LIBNAME).h $(INCLUDE_DIR)/
		$(COPY) $(LIBNAME).h $(LINCLUDE_DIR)/
		$(COPY) oauthlib.h $(INCLUDE_DIR)/
		$(COPY) oauthlib.h $(LINCLUDE_DIR)/
		ln -sf $(LIBRARY_DIR)/lib$(LIBNAME).so.1.0 $(LIBRARY_DIR)/lib$(LIBNAME).so
		ln -sf $(LIBRARY_DIR)/lib$(LIBNAME).so.1.0 $(LIBRARY_DIR)/lib$(LIBNAME).so.1
		ln -sf $(LLIBRARY_DIR)/lib$(LIBNAME).so.1.0 $(LLIBRARY_DIR)/lib$(LIBNAME).so
		ln -sf $(LLIBRARY_DIR)/lib$(LIBNAME).so.1.0 $(LLIBRARY_DIR)/lib$(LIBNAME).so.1
endif

uninstall:
ifeq ($(OS),Darwin)
	$(REMOVE) $(LLIBRARY_DIR)/$(LIBNAME).so.1.0
	$(REMOVE) $(LINCLUDE_DIR)/$(LIBNAME).h
	unlink $(LLIBRARY_DIR)/lib$(LIBNAME).so
	unlink $(LLIBRARY_DIR)/lib$(LIBNAME).so.1
else
	$(REMOVE) $(LLIBRARY_DIR)/$(LIBNAME).so.1.0
	$(REMOVE) $(LINCLUDE_DIR)/$(LIBNAME).h
	$(REMOVE) $(LIBRARY_DIR)/$(LIBNAME).so.1.0
	$(REMOVE) $(INCLUDE_DIR)/$(LIBNAME).h
	unlink $(LLIBRARY_DIR)/lib$(LIBNAME).so
	unlink $(LLIBRARY_DIR)/lib$(LIBNAME).so.1
	unlink $(LIBRARY_DIR)/lib$(LIBNAME).so
	unlink $(LIBRARY_DIR)/lib$(LIBNAME).so.1
endif