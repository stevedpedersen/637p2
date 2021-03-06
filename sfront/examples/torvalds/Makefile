
#    Sfront, a SAOL to C translator    
#    This file: Makefile for an sfront example
#
# Copyright (c) 2000-2006, Regents of the University of California
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:
#
#  Redistributions of source code must retain the above copyright
#  notice, this list of conditions and the following disclaimer.
#
#  Redistributions in binary form must reproduce the above copyright
#  notice, this list of conditions and the following disclaimer in the
#  documentation and/or other materials provided with the distribution.
#
#  Neither the name of the University of California, Berkeley nor the
#  names of its contributors may be used to endorse or promote products
#  derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#
#    Maintainer: John Lazzaro, lazzaro@cs.berkeley.edu


##
## this example demonstrates how to use the input_bus to read in an
## audio stream, do signal processing, and write it out. the example
## also shows how to do signal processing by reading a sample into 
## a wavetable. the task is cleaning up linus torvalds' voice. 
##

FILENAME = torvalds

SAOLFILE = $(FILENAME).saol
MP4FILE  = $(FILENAME).mp4

INFILE = english.wav
OUTFILE = output.wav

##
## Compiler optimization and debug options. Mac OS X users 
## should replace "gcc" with "cc".
## 

CC = gcc
OPT = -O3
CFLAGS = $(OPT)

SFRONT = sfront
PLAYER = play
CMP = cmp

OUTMODE = -aout $(OUTFILE)
INMODE = -ain $(INFILE)

## for INFILE/OUTFILE selections that need libraries

IOLINK = 

## for INFILE/OUTFILE std selection:  > foo < bar

REDIRECT = 

##
## makes a raw 16-bit signed integer audio file
##
 
$(OUTFILE): $(SAOLFILE) 
	$(SFRONT) $(OUTMODE) $(INMODE) -orc $(SAOLFILE) 
	$(CC) $(CFLAGS) sa.c -lm $(IOLINK) -o sa
	./sa $(REDIRECT)

## tests bitstream creation code, by creating an MP4 file, decoding
## it, and comparing audio out with audio created by original ASCII
## files. doesn't work with stdin/stdout.

mp4test	: $(OUTFILE)
	rm -rf $(MP4FILE) sa.c
	mv $(OUTFILE) safe
	$(SFRONT)  -orc $(SAOLFILE) -bitout $(MP4FILE)
	$(SFRONT) $(OUTMODE) $(INMODE) -bit $(MP4FILE)
	$(CC) $(CFLAGS) sa.c -lm $(IOLINK) -o sa
	./sa $(REDIRECT)
	$(CMP) $(OUTFILE) safe

## tests a new sfront against an old one. assume safe is the audio
## created by old sfront. 

compare	: $(OUTFILE)
	$(CMP) $(OUTFILE) safe

safe	: $(OUTFILE)
	cp $(OUTFILE) safe

## for performance testing
##

timing	: 
	/usr/bin/time -p ./sa $(REDIRECT)

##
##
##

## play the wav file

play  : $(OUTFILE)
	$(PLAYER) $(OUTFILE)

clean: 
	rm -rf sa.c sa audio a*.wav $(MP4FILE) $(OUTFILE) *.info *~ safe



