project(basi-syntax C)

cmake_minimum_required(VERSION 2.6)

set (wife Grace)
set (dog Stan)
message("${wife}, please take ${dog} for a walk")

set_property(SOURCE add.c PROPERTY Author Peter)
set_property(SOURCE mult.c PROPERTY Author John)
get_property(author_name SOURCE add.c PROPERTY Author)
message("The author of add.c is ${author_name}")

#===========================================================
# add library or include header file 
include_directories()
# add path of library
link_directories()
add_library(math STATIC add sub mult)
# complie
add_executable(calculator calc)
# connect project to library
target_link_libraries(calculator math)

#===========================================================
# debug option
set(CMAKE_BUILD_TYPE Debug)

# preprocessor definition
set_property(DIRECTORY PROPERTY COMPILE_DEFINITIONS TEST=1)
set_property(SOURCE add.c PROPERTY COMPILE_DEFINITIONS QUICKADD=1)

#===========================================================
# add_custom_command example 
project(custom_command)
cmake_minimum_required(VERSION 2.6)

set(input_data_file ${PROJECT_SOURCE_DIR}/data.dat)
set(output_c_file data.c)

add_custom_command(
    OUTPUT ${output_c_file}
    COMMAND /tools/bin/make-data-file
            < ${input_data_file}
            > ${output_c_file}
    DEPENDS ${input_data_file}
)

add_executable(print-data ${output_c_file})

#===========================================================
project(custom_target)
cmake_minimum_required(VERSION 2.6)

add_custom_target(
    print-city ALL
    COMMAND echo "Vancouver is a nice city"
    COMMAND echo "Even when it rains"
)

add_custom_target(
    print-time
    COMMAND echo "It is now 2:17pm"
)

add_custom_target(
    print-day
    COMMAND echo "Today is Monday"
)

add_dependencies(print-city print-time print-day)
#===========================================================
# if control flow example
set(my_val 1)
if(${my_val})
    message ("my_var is true")
else()
    message ("my_var is false")
endif()

# we can use NOT, AND, OR operation in cmake text
if(NOT my_var)
    ...
else()
    ...
endif()

# we can compare other value
if(${my_age} EQUAL 40)
    ...
endif()

if(EXISTS file1.txt)
    ...
endif()

# we can check create order between two files
if(file1.txt IS_NEWER_THAN file2.txt)
    ...
endif()

# we can use regular expression to check value
if(${symbol_name} MATCHES "^[a-z][a-z0-9]*$")
    ...
endif()
#===========================================================
# use macro example
project(macro)
cmake_minimum_required(VERSION 2.6)

macro(my_macro ARG1 ARG2 ARG3)
    message("The my_macro macro was passed the following arguments:")
    message("${ARG1} ${ARG2} ${ARG3}")
endmacro(my_macro)

my_macro(1 2 3)
my_macro(France Germany Russia)

#===========================================================
project(foreach)
cmake_minimum_required(VERSION 2.6)

foreach(source_file add.c sub.c mult.c calc.c)
    message("Calculation cksum for ${source_file}")
    add_custom_target(cksum-${source_file} ALL
        COMMAND cksum ${PROJECT_SOURCE_DIR}/${souce_file}
    )
endforeach(source_file)
#===========================================================
project(finding)
cmake_minimum_required(VERSION 3.0)

find_program(LS_PATH ls)
message("The path to the ls program is ${LS_PATH}")

find_file(STDIO_H_PATH stdio.h)
message("The path to the stdio.h file is ${STDIO_H_PATH}")

find_library(LIB_MATH_PATH m /usr/local/lib/usr/lib64)
message("The path to the math Library is {LIB_MATH_PATH}")
#===========================================================
project(find-perl)
cmake_minimum_required(VERSION 2.6)

include(FindPerl)
if(PERL_FOUND)
    execute_process(
        COMMAND ${PERL_EXECUTABLE} ${PROJECT_SOURCE_DIR} / 
        config.pl
    )
else()
    message(SEND_ERROR "There is no perl interpreter on this system")
endif()
#===========================================================

project(try-complie)
cmake_minimum_required(VERSION 2.6)

include(CheckFuctionExists)
include(CheckStructHasMember)

CHECK_FUNCTION_EXISTS(vsnprintf VSNPRINTF_EXISTS)
if(NOT VSNPRINTF_EXISTS)
    message(SEND_ERROR "vsnprintf not available on this build machine")
endif()

CHECK_STRUCT_HAS_MEMBER("struct rusage" ru_stime wait.h HAS_STIME)
if(NOT HAS_STIME)
    message(SEND_ERROR "ru_stime field not available in struct rusage")
endif()


#===========================================================
# source code in a single directory
project(scenario-1)
add_executable(calculator add sub mult calc)

#===========================================================
# source code in multiple directories
project(scenario-2)
cmake_minimum_required(VERSION 3.0)
add_subdirectory(libmath)
add_subdirectory(libprint)
add_subdirectory(calc)

# add library
add_library(Math clock letter number)
add_library(Print banner.c center.c normal.c)

add_executable(calculator calc.c)
# link libraries
target_link_libraries(calculator Math Print)

# ==========================================================
# Define a new compilation tool
project(scenario-3)
cmake_minimum_required(VERSION 2.6)

macro(mathcomp FUNC_NAME INPUT_FILE OUTPUT_FILE)
    execute_process(
        COMMAND /tools/bin/mathcomp -d ${INPUT_FILE}
        OUTPUT_VARIABLE DEFS
    )
    separate_arguments(DEFS)

    add_custom_command(
        OUTPUT ${OUTPUT_FILE}
        COMMAND /tools/bin/mathcomp -c -o ${OUTPUT_FILE} -f ${FUNC_NAME} ${INPUT_FILE}
    )
endmacro(mathcomp)

mathcomp(equations ${PROJECT_SOURCE_DIR}/equations.math equations.c)

add_executable(calculator calculator.c equations.c)
# ==========================================================
# Build with multiple variants
project(scenario-4)

set(PLATFORM i386 CACHE STRING "CPU Type : i386, powerrpc or alpha")

if(NOT ${PLATFORM} MATCHES "^(i386|powerpc|alpha)$*")
    message(SEND_ERROR "INVALID PLATFORM")
endif()
message("Compile code ${PLATFORM}")
set(CMAKE_C_COMPILER /tools/bin/gcc-${PLATFORM})
add_executable(calculator add mul sub calc)












