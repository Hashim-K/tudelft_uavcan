#
# Copyright (C) 2019 Freek van Tienen <freek.v.tienen@gmail.com>
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#
#

### DEBUG
# USE_OPT = -g -ggdb -O0 -fomit-frame-pointer -falign-functions=16
# USE_LTO = no

# Compiler options here.
ifeq ($(USE_OPT),)
  USE_OPT = -Os -fomit-frame-pointer -falign-functions=16
endif

# C specific options here (added to USE_OPT).
ifeq ($(USE_COPT),)
  USE_COPT =
endif

# C++ specific options here (added to USE_OPT).
ifeq ($(USE_CPPOPT),)
  USE_CPPOPT = -fno-rtti
endif

# Enable this if you want the linker to remove unused code and data.
ifeq ($(USE_LINK_GC),)
  USE_LINK_GC = yes
endif

# Linker extra options here.
ifeq ($(USE_LDOPT),)
  USE_LDOPT =
endif

# Enable this if you want link time optimizations (LTO).
ifeq ($(USE_LTO),)
  USE_LTO = yes
endif

# If enabled, this option allows to compile the application in THUMB mode.
ifeq ($(USE_THUMB),)
  USE_THUMB = yes
endif

# Enable this if you want to see the full log while compiling.
ifeq ($(USE_VERBOSE_COMPILE),)
  USE_VERBOSE_COMPILE = no
endif

# If enabled, this option makes the build process faster by not compiling
# modules not used in the current configuration.
ifeq ($(USE_SMART_BUILD),)
  USE_SMART_BUILD = yes
endif

#
# Build global options
##############################################################################

##############################################################################
# Architecture or project specific options
#

# Stack size to be allocated to the Cortex-M process stack. This stack is
# the stack used by the main() thread.
ifeq ($(USE_PROCESS_STACKSIZE),)
  USE_PROCESS_STACKSIZE = 0xC00
endif

# Stack size to the allocated to the Cortex-M main/exceptions stack. This
# stack is used for processing interrupts and exceptions.
ifeq ($(USE_EXCEPTIONS_STACKSIZE),)
  USE_EXCEPTIONS_STACKSIZE = 0x400
endif

# Enables the use of FPU (no, softfp, hard).
ifeq ($(USE_FPU),)
  USE_FPU = no
endif

# FPU-related options.
ifeq ($(USE_FPU_OPT),)
  USE_FPU_OPT = -mfloat-abi=$(USE_FPU) -mfpu=fpv4-sp-d16
endif

#
# Architecture or project specific options
##############################################################################

##############################################################################
# Project, target, sources and paths
#

# Define project name here
PROJECT = tuc_node

# Target settings.
MCU  ?= cortex-m3

# Version information
GIT_DESC := $(shell git describe --dirty --always --tags 2>/dev/null)
_SOFT_HASH := $(shell git log -1 --pretty=format:%h 2> /dev/null)
SOFT_HASH := $(if $(_SOFT_HASH),$(_SOFT_HASH),0)
SOFT_VER := $(shell echo $(GIT_DESC) | sed -e 's/[^0-9.]*\([0-9.]*\).*/\1/' -e 't' -e 's/.*/0.0/')
SOFT_VER_MAJOR := $(shell echo $(GIT_DESC) | sed -e 's/v\([0-9]*\).*/\1/' -e 't' -e 's/.*/0/')
SOFT_VER_MINOR := $(shell echo $(GIT_DESC) | sed -e 's/v[0-9]*.\([0-9]*\).*/\1/' -e 't' -e 's/.*/0/')

# Imported source files and paths.
CHIBIOS  := ../lib/chibios
BOARDDIR := ../boards
CONFDIR  := ./cfg
BUILDDIR := ../build/$(BOARD)/node
DEPDIR   := ./.dep
CANARDDIR  := ../lib/canard
DSDLC_DIR  := ../lib/dsdlc_generated
DSDLC_SRC_DIR  := $(DSDLC_DIR)/src
MAVLINK_DIR := ../lib/mavlink

# Board files.
include $(BOARDDIR)/$(BOARD)/board.mk
# Licensing files.
include $(CHIBIOS)/os/license/license.mk
# Startup files.
include $(CHIBIOS)/os/common/startup/ARMCMx/compilers/GCC/mk/$(CHIBIOS_BOARD_STARTUP)
# HAL-OSAL files (optional).
include $(CHIBIOS)/os/hal/hal.mk
include $(CHIBIOS)/os/hal/ports/STM32/$(CHIBIOS_BOARD_PLATFORM)
include $(CHIBIOS)/os/hal/osal/rt-nil/osal.mk
# RTOS files (optional).
include $(CHIBIOS)/os/rt/rt.mk
include $(CHIBIOS)/os/common/ports/ARMv7-M/compilers/GCC/mk/port.mk
# Other files (optional).
include $(CHIBIOS)/os/hal/lib/streams/streams.mk

# Define linker script file here
LDSCRIPT= $(BOARDDIR)/$(BOARD)/node.ld

# C sources that can be compiled in ARM or THUMB mode depending on the global
# setting.
CSRC = $(ALLCSRC) \
       $(CHIBIOS)/os/various/syscalls.c \
       $(TESTSRC) \
	     $(CANARDDIR)/canard.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.dynamic_node_id.Allocation.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.debug.LogLevel.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.debug.LogMessage.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.GetNodeInfo_req.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.GetNodeInfo_res.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.NodeStatus.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.SoftwareVersion.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.HardwareVersion.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.file.BeginFirmwareUpdate_req.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.file.BeginFirmwareUpdate_res.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.file.Path.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.file.Read_req.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.file.Read_res.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.GetSet_req.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.GetSet_res.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.Value.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.Empty.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.NumericValue.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.ExecuteOpcode_req.c \
       $(DSDLC_SRC_DIR)/uavcan.protocol.param.ExecuteOpcode_res.c \
       $(DSDLC_SRC_DIR)/uavcan.tunnel.Broadcast.c \
       $(DSDLC_SRC_DIR)/uavcan.tunnel.Call_req.c \
       $(DSDLC_SRC_DIR)/uavcan.tunnel.Call_res.c \
       $(DSDLC_SRC_DIR)/uavcan.tunnel.Protocol.c \
       $(DSDLC_SRC_DIR)/uavcan.Timestamp.c \
       $(DSDLC_SRC_DIR)/uavcan.CoarseOrientation.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.esc.Status.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.esc.RawCommand.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.device.Temperature.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.actuator.Status.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.actuator.ArrayCommand.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.actuator.Command.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.range_sensor.Measurement.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.power.BatteryInfo.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.power.CircuitStatus.c \
       $(DSDLC_SRC_DIR)/uavcan.equipment.fuelcell.Status.c \
	     flash.c config.c uavcan.c node.c servos.c esc_telem.c adcs.c \
       faulhaber_ctrl.c drs_parachute.c tfmini.c ie_fuelcell.c feetech_sts.c \
       main.c

# C++ sources that can be compiled in ARM or THUMB mode depending on the global
# setting.
CPPSRC = $(ALLCPPSRC)

# C sources to be compiled in ARM mode regardless of the global setting.
# NOTE: Mixing ARM and THUMB mode enables the -mthumb-interwork compiler
#       option that results in lower performance and larger code size.
ACSRC =

# C++ sources to be compiled in ARM mode regardless of the global setting.
# NOTE: Mixing ARM and THUMB mode enables the -mthumb-interwork compiler
#       option that results in lower performance and larger code size.
ACPPSRC =

# C sources to be compiled in THUMB mode regardless of the global setting.
# NOTE: Mixing ARM and THUMB mode enables the -mthumb-interwork compiler
#       option that results in lower performance and larger code size.
TCSRC =

# C sources to be compiled in THUMB mode regardless of the global setting.
# NOTE: Mixing ARM and THUMB mode enables the -mthumb-interwork compiler
#       option that results in lower performance and larger code size.
TCPPSRC =

# List ASM source files here
ASMSRC = $(ALLASMSRC)
ASMXSRC = $(ALLXASMSRC)

INCDIR = $(CONFDIR) $(ALLINC) $(TESTINC) $(CANARDDIR) $(DSDLC_DIR)/include $(MAVLINK_DIR)

# Define C warning options here.
CWARN = -Wall -Wextra -Wundef -Wstrict-prototypes -Wno-address-of-packed-member

# Define C++ warning options here.
CPPWARN = -Wall -Wextra -Wundef

#
# Project, target, sources and paths
##############################################################################

##############################################################################
# Start of user section
#

# List all user C define here, like -D_DEBUG=1
UDEFS = -DCANARD_ENABLE_CUSTOM_BUILD_CONFIG=1
UDEFS += -DGIT_DESC=$(GIT_DESC) -DSOFT_HASH=0x$(SOFT_HASH)
UDEFS += -DSOFT_VER=$(SOFT_VER) -DSOFT_VER_MAJOR=$(SOFT_VER_MAJOR) -DSOFT_VER_MINOR=$(SOFT_VER_MINOR)
#UDEFS += -DCH_DBG_SYSTEM_STATE_CHECK=TRUE -DCH_DBG_ENABLE_CHECKS=TRUE -DCH_DBG_ENABLE_ASSERTS=TRUE
#UDEFS += -DCH_DBG_ENABLE_STACK_CHECK=TRUE -DCH_DBG_FILL_THREADS=TRUE

# Define ASM defines here
UADEFS =

# List all user directories here
UINCDIR =

# List the user directory to look for the libraries here
ULIBDIR =

# List all user libraries here
ULIBS = -lm

#
# End of user section
##############################################################################

##############################################################################
# Common rules
#

RULESPATH = $(CHIBIOS)/os/common/startup/ARMCMx/compilers/GCC/mk
include $(RULESPATH)/arm-none-eabi.mk
include $(RULESPATH)/rules.mk

#
# Common rules
##############################################################################

##############################################################################
# Custom rules
#

#
# Custom rules
##############################################################################