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
PLATFORM = HOST 

# Output files
TARGET := c1m2

# Architectures Specific Flags
ifeq ($(PLATFORM),MSP432)
	LINKER_FILE = ../msp432p401r.lds
	CPU = cortex-m4
	ARCH = armv7e-m
	SPECS = nosys.specs
	FPU = fpv4-sp-d16
	FLOAT_ABI = hard 
else
	CPU = $(shell cpu)
	ARCH = $(shell arch)
endif

# Compiler Flags and Defines
GFLAGS = -Wall -Werror -g -O0 -std=c99

# Compiler and Linker
ifeq ($(PLATFORM),MSP432)
    CC = arm-none-eabi-gcc
    SOURCES = $(SOURCES_COMMON) $(SOURCES_MSP432)
    INCLUDES = $(INCLUDES_COMMON) $(INCLUDES_MSP432)
    LDFLAGS = -T $(LINKER_FILE) -Wl,-Map=$(TARGET).map
	CFLAGS = -mcpu=$(CPU) -mthumb -march=$(ARCH) -mfloat-abi=$(FLOAT_ABI) \
    -mfpu=$(FPU) --specs=$(SPECS) $(GFLAGS)

else
    CC = gcc
    SOURCES = $(SOURCES_COMMON)
    INCLUDES = $(INCLUDES_COMMON)
    LDFLAGS =
    CFLAGS = $(GFLAGS) -DHOST

endif

# Rules
.PHONY: build compile-all clean

#------------------------------------------------------------------------------
# Target: build
# Prerequisites: All object files.
# Output: All files are compiled and generates the final executable <file>.out, 
#------------------------------------------------------------------------------
build: $(SOURCES:.c=.o)
	$(CC) $(CFLAGS) $(INCLUDES) $(SOURCES:.c=.o) $(LDFLAGS) -o c1m2.out

#------------------------------------------------------------------------------
# Target: compile-all
# Prerequisites: All source files.
# Output: All object files are compiled without linking and the corresponding 
# <files>.o are generated.
#------------------------------------------------------------------------------
compile-all: $(SOURCES:.c=.o)
	$(CC) $(CFLAGS) $(INCLUDES) -c $(SOURCES)

#------------------------------------------------------------------------------
# Target: <file>.o
# Prerequisites: <file>.c 
# Output: <file>.o
#------------------------------------------------------------------------------
%.o: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -MMD -c $< -o $@

#------------------------------------------------------------------------------
# Target: <file>.i
# Prerequisites: <file>.c 
# Output: <file>.i and <file>.d
#------------------------------------------------------------------------------
%.i: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -E $< -o $@

#------------------------------------------------------------------------------
# Target: <file>.asm
# Prerequisites: <file>.c, <file>.out
# Output: <file>.asm
#------------------------------------------------------------------------------
%.asm: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -S $< -o $@

#------------------------------------------------------------------------------
# Target: clean
# Prerequisites: None.
# Output: Remove all compiled objects, preprocessed outputs, assembly outputs,
#		  executable files and build output files
#------------------------------------------------------------------------------
clean:
	rm -f *.o *.out *.map *.d *.i *.asm