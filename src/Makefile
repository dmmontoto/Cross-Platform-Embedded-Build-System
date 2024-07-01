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

# Platform Overrides
PLATFORM ?= HOST 

# Compiler and Linker
ifeq ($(PLATFORM),MSP432)
    CC = arm-none-eabi-gcc
    SOURCES = $(SOURCES_COMMON) $(SOURCES_MSP432)
    INCLUDES = $(INCLUDES_COMMON) $(INCLUDES_MSP432)
    LDFLAGS = -T ../msp432p401r.lds -Wl,-Map=c1m2.map
    CFLAGS = -Wall -Werror -g -O0 -std=c99 -mcpu=cortex-m4 -mthumb \
	         -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 \
			 --specs=nosys.specs -DMSP432

else
    CC = gcc
    SOURCES = $(SOURCES_COMMON)
    INCLUDES = $(INCLUDES_COMMON)
    LDFLAGS =
    CFLAGS = -Wall -Werror -g -O0 -std=c99 -DHOST

endif

# Rules
.PHONY: build compile-all clean

build: $(SOURCES:.c=.o)
	$(CC) $(CFLAGS) $(INCLUDES) $(SOURCES:.c=.o) $(LDFLAGS) -o c1m2.out

compile-all: $(SOURCES:.c=.o)
	$(CC) $(CFLAGS) $(INCLUDES) -c $(SOURCES)

%.o: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -MMD -c $< -o $@

%.i: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -E $< -o $@

%.asm: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -S $< -o $@

clean:
	rm -f *.o *.out *.map *.d *.i *.asm