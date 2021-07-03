##############################################################################
BUILD = build
BIN = free_dap

##############################################################################
.PHONY: all directory clean size

CC = arm-none-eabi-gcc
OBJCOPY = arm-none-eabi-objcopy
SIZE = arm-none-eabi-size
UF2CONV = python ../../../tools/uf2conv.py

#ifeq ($(OS), Windows_NT)
#  MKDIR = gmkdir
#else
  MKDIR = mkdir
#endif

CFLAGS += -W -Wall -Wextra --std=gnu99 -O2
CFLAGS += -fno-diagnostics-show-caret
CFLAGS += -fdata-sections -ffunction-sections
CFLAGS += -funsigned-char -funsigned-bitfields
CFLAGS += -mcpu=cortex-m0plus -mthumb
CFLAGS += -MD -MP -MT $(BUILD)/$(*F).o -MF $(BUILD)/$(@F).d

LDFLAGS += -mcpu=cortex-m0plus -mthumb
LDFLAGS += -Wl,--gc-sections
LDFLAGS += -Wl,--script=../linker/samd21g18.ld

INCLUDES += \
  -I../include \
  -I../../.. \
  -I..

SRCS += \
  ../main.c \
  ../uart.c \
  ../usb.c \
  ../usb_std.c \
  ../usb_cdc.c \
  ../usb_hid.c \
  ../usb_descriptors.c \
  ../startup_samd21.c \
  ../../../dap.c

DEFINES += \
  -D__SAMD21J18A__ \
  -DDONT_USE_CMSIS_INIT \
  -DF_CPU=48000000 

CFLAGS += $(INCLUDES) $(DEFINES)

OBJS = $(addprefix $(BUILD)/, $(notdir %/$(subst .c,.o, $(SRCS))))

all: directory $(BUILD)/$(BIN).elf $(BUILD)/$(BIN).hex $(BUILD)/$(BIN).bin $(BUILD)/$(BIN).uf2 size

$(BUILD)/$(BIN).elf: $(OBJS)
	@echo LD $@
	@$(CC) $(LDFLAGS) $(OBJS) $(LIBS) -o $@

$(BUILD)/$(BIN).hex: $(BUILD)/$(BIN).elf
	@echo OBJCOPY $@
	@$(OBJCOPY) -O ihex $^ $@

$(BUILD)/$(BIN).bin: $(BUILD)/$(BIN).elf
	@echo OBJCOPY $@
	@$(OBJCOPY) -O binary $^ $@

$(BUILD)/$(BIN).uf2: $(BUILD)/$(BIN).bin
	@echo UF2CONV $@
	@$(UF2CONV) -c -b 0x2000 -o $@ $^

%.o:
	@echo CC $@
	@$(CC) $(CFLAGS) $(filter %/$(subst .o,.c,$(notdir $@)), $(SRCS)) -c -o $@

directory:
	@$(MKDIR) -p $(BUILD)

size: $(BUILD)/$(BIN).elf
	@echo size:
	@$(SIZE) -t $^

clean:
	@echo clean
	@-rm -rf $(BUILD)

-include $(wildcard $(BUILD)/*.d)

