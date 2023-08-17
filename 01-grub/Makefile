#Assembler
ASM = /usr/bin/nasm
#GCC
GCC = /usr/bin/gcc
# linker
LD = /usr/bin/ld
# grub iso creator
GRUB = /usr/bin/grub-mkrescue
# sources
SRC = kernel
ASM_SRC = boot
# objects
OBJ = obj
ASM_OBJ = $(OBJ)/boot
CONFIG = ./cfg
OUT = out
INC = ./include
INCLUDE=-I$(INC)

# assembler flags
ASM_FLAGS = -f elf32
# compiler flags
CC_FLAGS = $(INCLUDE) $(DEFINES) -m32 -std=gnu99 -ffreestanding -Wall -Wextra
# linker flags, for linker add linker.ld file too
LD_FLAGS = -m elf_i386 -T $(CONFIG)/linker.ld -nostdlib

# target file to create in linking
TARGET=$(OUT)/kernel.bin

MKDIR = mkdir -p
CP = cp -f
DEFINES=

# qemu
QEMU= qemu-system-i386
QEMU_FLAGS= -cdrom out/kernel.iso

# iso file target to create
TARGET_ISO=$(OUT)/kernel.iso
ISO_DIR=$(OUT)/isodir

OBJECTS=$(ASM_OBJ)/boot.o\
		$(OBJ)/kernel.o\
		
all: $(OBJECTS)
	@printf "[ linking... ]\n"
	$(LD) $(LD_FLAGS) -o $(TARGET) $(OBJECTS)
	grub-file --is-x86-multiboot $(TARGET)
	@printf "\n"
	@printf "[ building ISO... ]\n"
	$(MKDIR) $(ISO_DIR)/boot/grub
	$(CP) $(TARGET) $(ISO_DIR)/boot/
	$(CP) $(CONFIG)/grub.cfg $(ISO_DIR)/boot/grub/
	$(GRUB) -o $(TARGET_ISO) $(ISO_DIR)
	rm -f $(TARGET)

$(ASM_OBJ)/boot.o : $(ASM_SRC)/boot.asm
	@printf "[ $(ASM_SRC)/boot.asm ]\n"
	$(ASM) $(ASM_FLAGS) $(ASM_SRC)/boot.asm -o $(ASM_OBJ)/boot.o
	@printf "\n"

$(OBJ)/kernel.o : $(SRC)/kernel.c
	@printf "[ $(SRC)/kernel.c ]\n"
	$(CC) $(CC_FLAGS) -c $(SRC)/kernel.c -o $(OBJ)/kernel.o
	@printf "\n"

clean:
	rm -f $(OBJ)/*.o
	rm -f $(ASM_OBJ)/*.o
	rm -rf $(OUT)/*

run:
	$(QEMU) $(QEMU_FLAGS)
