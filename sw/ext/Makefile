# Hey Emacs, this is a -*- makefile -*-
#
#   Copyright (C) 2012 The Paparazzi Team
#
# This file is part of paparazzi.
#
# paparazzi is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
#
# paparazzi is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with paparazzi; see the file COPYING.  If not, write to
# the Free Software Foundation, 59 Temple Place - Suite 330,
# Boston, MA 02111-1307, USA.

# The default is to produce a quiet echo of compilation commands
# Launch with "make Q=''" to get full echo

Q=@

PAPARAZZI_SRC=../..
EXT_DIR=$(PAPARAZZI_SRC)/sw/ext

#
# find compiler toolchain
#
include $(PAPARAZZI_SRC)/conf/Makefile.arm-embedded-toolchain


all: libopencm3  chibios fatfs

# update (and init if needed) all submodules
update_submodules:
	$(Q)if [ -d $(PAPARAZZI_SRC)/.git ]; then \
		cd $(PAPARAZZI_SRC) && git submodule update --init; \
	fi

# update (and init if needed) a specific submodule
%.update:
	$(Q)if [ -d $(PAPARAZZI_SRC)/.git ]; then \
		cd $(PAPARAZZI_SRC) && git submodule update --init sw/ext/$*; \
	fi

# only build currentl checkout of libopencm3
libopencm3.build:
	$(Q)$(MAKE) -C libopencm3 lib PREFIX=$(PREFIX) TARGETS="stm32/f1"
	$(Q)$(MAKE) -C libopencm3 lib PREFIX=$(PREFIX) FP_FLAGS="-mfloat-abi=softfp -mfpu=fpv4-sp-d16" TARGETS="stm32/f4"

# update and then build libopencm3
libopencm3: libopencm3.update libopencm3.build


# update libopencm3, then build it
: libopencm3.update 

chibios: chibios.update

fatfs: fatfs.update

clean:
	$(Q)if [ -f libopencm3/Makefile ]; then \
		$(MAKE) -C $(EXT_DIR)/libopencm3 clean; \
	fi


.NOTPARALLEL: libopencm3 
.PHONY: all clean update_submodules libopencm3  chibios fatfs luftboot_flash libopencm3.build
