CC                    := clang
BIN		                := fuzz_demo

BUILD_DIR             := ./build
BUILD_DIR_ABS         := $(abspath $(BUILD_DIR))

FREERTOS_DIR_REL      := ../../../FreeRTOS
FREERTOS_DIR          := $(abspath $(FREERTOS_DIR_REL))

FREERTOS_PLUS_DIR_REL := ../../../FreeRTOS-Plus
FREERTOS_PLUS_DIR     := $(abspath $(FREERTOS_PLUS_DIR_REL))

KERNEL_DIR            := ${FREERTOS_DIR}/Source

INCLUDE_DIRS          := -I.
INCLUDE_DIRS          += -I./Trace_Recorder_Configuration
INCLUDE_DIRS          += -I${KERNEL_DIR}/include
INCLUDE_DIRS          += -I${KERNEL_DIR}/portable/ThirdParty/GCC/Posix
INCLUDE_DIRS          += -I${KERNEL_DIR}/portable/ThirdParty/GCC/Posix/utils
INCLUDE_DIRS          += -I${FREERTOS_DIR}/Demo/Common/include
INCLUDE_DIRS          += -I${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/include
INCLUDE_DIRS          += -I${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/config
INCLUDE_DIRS          += -I${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/streamports/File/include
INCLUDE_DIRS          += -I${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/streamports/File/config
INCLUDE_DIRS          += -I${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/kernelports/FreeRTOS/include
INCLUDE_DIRS          += -I${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/kernelports/FreeRTOS/

# SOURCE_FILES          := $(wildcard *.c)
SOURCE_FILES          := main_fuzz.c
SOURCE_FILES          += $(wildcard ${FREERTOS_DIR}/Source/*.c)
# Memory manager (use malloc() / free() )
SOURCE_FILES          += ${KERNEL_DIR}/portable/MemMang/heap_3.c
# posix port
SOURCE_FILES          += ${KERNEL_DIR}/portable/ThirdParty/GCC/Posix/utils/wait_for_event.c
SOURCE_FILES          += ${KERNEL_DIR}/portable/ThirdParty/GCC/Posix/port.c

# Demo library.
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/AbortDelay.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/BlockQ.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/blocktim.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/countsem.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/death.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/dynamic.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/EventGroupsDemo.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/flop.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/GenQTest.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/integer.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/IntSemTest.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/MessageBufferAMP.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/MessageBufferDemo.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/PollQ.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/QPeek.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/QueueOverwrite.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/QueueSet.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/QueueSetPolling.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/recmutex.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/semtest.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/StaticAllocation.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/StreamBufferDemo.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/StreamBufferInterrupt.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/TaskNotify.c
SOURCE_FILES          += ${FREERTOS_DIR}/Demo/Common/Minimal/TimerDemo.c



CFLAGS                :=    -ggdb3
LDFLAGS               :=    -ggdb3 -pthread
CPPFLAGS              :=    $(INCLUDE_DIRS) -DBUILD_DIR=\"$(BUILD_DIR_ABS)\"
CPPFLAGS              +=    -D_WINDOWS_

ifeq ($(TRACE_ON_ENTER),1)
  CPPFLAGS              += -DTRACE_ON_ENTER=1
else
  CPPFLAGS              += -DTRACE_ON_ENTER=0
endif

ifeq ($(COVERAGE_TEST),1)
  CPPFLAGS              += -DprojCOVERAGE_TEST=1
  CPPFLAGS              += -DprojENABLE_TRACING=0
  CFLAGS                += -Werror
else
  ifeq ($(NO_TRACING),1)
    CPPFLAGS              += -DprojENABLE_TRACING=0
  else
    CPPFLAGS              += -DprojENABLE_TRACING=1
    # Trace Library.
    SOURCE_FILES          += ${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/kernelports/FreeRTOS/trcKernelPort.c
    SOURCE_FILES          += $(wildcard ${FREERTOS_PLUS_DIR}/Source/FreeRTOS-Plus-Trace/*.c )
  endif
  CPPFLAGS              += -DprojCOVERAGE_TEST=0
endif

ifdef PROFILE
  CFLAGS              +=   -pg  -O0
  LDFLAGS             +=   -pg  -O0
else
  CFLAGS              +=   -O3
  LDFLAGS             +=   -O3
endif

ifdef SANITIZE_ADDRESS
  CFLAGS              +=   -fsanitize=address -fsanitize=alignment
  LDFLAGS             +=   -fsanitize=address -fsanitize=alignment
endif

ifdef SANITIZE_LEAK
  LDFLAGS             +=   -fsanitize=leak
endif

ifdef SANITIZE_FUZZER
  CFLAGS              +=   -fsanitize=fuzzer
  LDFLAGS             +=   -fsanitize=fuzzer
endif

ifeq ($(USER_DEMO),BLINKY_DEMO)
  CPPFLAGS            +=   -DUSER_DEMO=0
endif

ifeq ($(USER_DEMO),FULL_DEMO)
  CPPFLAGS            +=   -DUSER_DEMO=1
endif

ifeq ($(USER_DEMO),FUZZ_DEMO)
  CPPFLAGS            +=   -DUSER_DEMO=2
endif

ifdef NO_STRINGS
  CFLAGS              +=  -DNO_STRINGS
endif

ifeq ($(FUZZ_TEST),TEST_1)
  CPPFLAGS            +=   -DTEST_1=1
endif

ifeq ($(FUZZ_TEST),TEST_2)
  CPPFLAGS            +=   -DTEST_2=1
endif

ifeq ($(FUZZ_TEST),TEST_3)
  CPPFLAGS            +=   -DTEST_3=1
endif

OBJ_FILES = $(SOURCE_FILES:%.c=$(BUILD_DIR)/%.o)

DEP_FILE = $(OBJ_FILES:%.o=%.d)

${BIN} : $(BUILD_DIR)/$(BIN)

${BUILD_DIR}/${BIN} : ${OBJ_FILES}
	-mkdir -p ${@D}
	$(CC) $^ ${LDFLAGS} -o $@

-include ${DEP_FILE}

${BUILD_DIR}/%.o : %.c Makefile
	-mkdir -p $(@D)
	$(CC) $(CPPFLAGS) $(CFLAGS) -MMD -c $< -o $@

.PHONY: clean

clean:
	-rm -rf $(BUILD_DIR)


GPROF_OPTIONS := --directory-path=$(INCLUDE_DIRS)
profile:
	gprof -a -p --all-lines $(GPROF_OPTIONS) $(BUILD_DIR)/$(BIN) $(BUILD_DIR)/gmon.out > $(BUILD_DIR)/prof_flat.txt
	gprof -a --graph $(GPROF_OPTIONS) $(BUILD_DIR)/$(BIN) $(BUILD_DIR)/gmon.out > $(BUILD_DIR)/prof_call_graph.txt

