#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk


PLATFORM = HOST
TARGET = c1m2

# Architectures Specific Flags
LINKER_FILE = msp432p401r.lds
CPU = cortex-m4
ARCH = thumb
SPECS = nosys.specs

# Compiler Flags and Defines
LD = arm-none-eabi-ld
LDFLAGS = 
CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -Wall -Werror -g -std=c99 \
	 -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16
CPPFLAGs = -D$(PLATFORM)

ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	LDFLAGS = -Wl, -Map=$(TARGET).map -T $(LINKER_FILE)
else
	CC = gcc
endif

OBJS = $(SRCS:.c=.o)

%.i : %.c
	$(CC) -E $(CPPFLAGs) -o $< $@

%.asm : %.c
	$(LD) -S -o $< $@
	$(OBJDUMP) $@

%.d : %.c
	$(CC) -MM $(CPPFLAGs) $< -o $@

%.o : %.c
	$(CC) -c $< $(CFLAGS) -o $@

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map
