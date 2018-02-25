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
TARGET = c1m2

# Architectures Specific Flags
ifeq ($(PLATFORM),HOST)
else ifeq ($(PLATFORM),MSP432)
	LINKER_FILE = ../msp432p401r.lds
	CPU = cortex-m4
	ARCH = thumb
	SPECS = nosys.specs
else
	$(error "Unsupported platform")
endif

# Compiler Flags and Defines
CFLAGS = -Wall -Werror -g -O0 -std=c99 -D$(PLATFORM)
LDFLAGS = -Wl,-Map=c1m2.map
CPPFLAGS = $(INCLUDES)

ifeq ($(PLATFORM),HOST)
	CC = gcc
	LD = ld
else ifeq ($(PLATFORM),MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LDFLAGS += -T $(LINKER_FILE)
	CFLAGS += -mcpu=$(CPU) -m$(ARCH) -march=armv7e-m -mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=$(SPECS)
else
	$(error "Unsupported platform")
endif

OBJS = $(SOURCES:.c=.o)

%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) -c $< -o $@

.PHONY: build
build: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).map $(TARGET).out
