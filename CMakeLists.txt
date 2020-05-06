cmake_minimum_required(VERSION 3.16)
project("C-Capnproto")

option(CCNP_FIND_CAPNP "Try to find capnp if not a target" OFF)

if(0)
if(NOT TARGET capnp)
    if(CCNP_FIND_CAPNP)
        find_program(capnp_PATH capnp)
        if(NOT capnp_PATH)
            message(FATAL_ERROR "Unable to find capnp")
        else()
            add_executable(capnp IMPORTED)
            set_target_properties(capnp PROPERTIES
                IMPORTED_LOCATION ${capnp_PATH}
            )
        endif()
    else()
        message(FATAL_ERROR "capnp is not a target and CCNP_FIND_CAPNP not set")
    endif()
endif()
endif()

set(libcapnpc_SRCS
    lib/capn-malloc.c
    lib/capn-stream.c
    lib/capn.c
)

set(capnpc_SRCS
    compiler/capnpc-c.c
    compiler/schema.capnp.c
    compiler/str.c
)
add_library(capnp_c STATIC ${libcapnpc_SRCS})
target_include_directories(capnp_c PUBLIC "lib")

add_executable(capnp-c ${capnpc_SRCS})
target_link_libraries(capnp-c capnp_c)
get_filename_component(capnp_include compiler ABSOLUTE)
set_target_properties(capnp-c PROPERTIES capnp_include_path ${capnp_include})
install(TARGETS capnp_c capnp-c EXPORT capnp-c-export)
