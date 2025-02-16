 #########################################################################

# CMAKE MINIMAL VERSION
cmake_minimum_required (VERSION 3.8)

# PROJECT PROPERTIES
project ("nCPU" LANGUAGES CXX)

#########################################################################

# ASSEMBLY OPTIONS

if(NOT CMAKE_CXX_EXTENSIONS)
    set(CMAKE_CXX_EXTENSIONS OFF)
endif()

add_compile_options(
    -Wall
    -Wpedantic

    -Wno-int-to-pointer-cast
    -Wno-variadic-macros
    -Wno-unused-variable
    -Wno-unused-parameter
    -Wno-unused-function
    -Wno-sign-compare
    -Wno-misleading-indentation
    -Wno-unused-but-set-variable
    -Wno-format
)

#########################################################################

# TEXT LIBRARY

set(TEXT_LIB_PATH)
set(TEXT_LIB_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../libtext)

find_library(TEXT_LIB_PATH NAMES text PATHS ${CMAKE_CURRENT_BINARY_DIR}/.. ${CMAKE_CURRENT_BINARY_DIR})

if (NOT TEXT_LIB_PATH)
    set(TEXT_LIB_BIN_DIR ${CMAKE_CURRENT_BINARY_DIR}/libtext)
    add_subdirectory(${TEXT_LIB_SRC_DIR} ${TEXT_LIB_BIN_DIR})
    set(TEXT_LIB_PATH text)
endif()

# STACK

set(STACK_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/libstack)
set(STACK_LIB_SRC ${CMAKE_CURRENT_SOURCE_DIR}/../libstack)
add_subdirectory(${STACK_LIB_SRC} ${STACK_LIB_DIR})
set(STACK_LIB_PATH stack)

# nCPU

set(NCPU_NAME "nCPU")

set(NCPU_SRC
    nCPU/main.cpp
    nCPU/cpu.cpp
)

add_executable(${NCPU_NAME} ${NCPU_SRC})

target_include_directories(${NCPU_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(${NCPU_NAME} PRIVATE ${STACK_LIB_SRC} ${TEXT_LIB_SRC_DIR})
target_link_libraries(${NCPU_NAME} PRIVATE ${STACK_LIB_PATH} ${TEXT_LIB_PATH})

# ASSEMBLER COMPILER

set(ASM_NAME "asm_compiler")

set(ASM_SRC
    asm_compiler/main.cpp
    asm_compiler/compile.cpp
)

add_executable(${ASM_NAME} ${ASM_SRC})

target_include_directories(${ASM_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(${ASM_NAME} PRIVATE ${STACK_LIB_SRC} ${TEXT_LIB_SRC_DIR})
target_link_libraries(${ASM_NAME} PRIVATE ${STACK_LIB_PATH} ${TEXT_LIB_PATH})

#########################################################################

# DISASSEMBLER

set(DISASM_NAME "disasm")

set(DISASM_SRC
    disasm_compiler/main.cpp
    disasm_compiler/disasm_func.cpp
)

add_executable(${DISASM_NAME} ${DISASM_SRC})

target_include_directories(${DISASM_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(${DISASM_NAME} PRIVATE ${STACK_LIB_SRC} ${TEXT_LIB_SRC_DIR})
target_link_libraries(${DISASM_NAME} PRIVATE ${STACK_LIB_PATH} ${TEXT_LIB_PATH})

#########################################################################

