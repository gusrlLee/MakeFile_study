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














