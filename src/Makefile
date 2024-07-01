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
ifeq ($(PLATFORM),HOST)
    CC = gcc
    SOURCES = $(SOURCES_COMMON)
    INCLUDES = $(INCLUDES_COMMON)
    LDFLAGS =
    CFLAGS = -Wall -Werror -g -O0 -std=c99 -DHOST

else ifeq ($(PLATFORM),MSP432)
    CC = arm-none-eabi-gcc
    SOURCES = $(SOURCES_COMMON) $(SOURCES_MSP432)
    INCLUDES = $(INCLUDES_COMMON) $(INCLUDES_MSP432)
    LDFLAGS = -T ../msp432p401r.lds
    CFLAGS = -Wall -Werror -g -O0 -std=c99 -mcpu=cortex-m4 -mthumb \
	         -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 \ 
			 --specs=nosys.specs -DMSP432

endif
