cmake_minimum_required(VERSION 3.16)
project("C-Capnproto")

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
install(FILES 
    lib/capnp_c.h
    TYPE INCLUDE
)
install(FILES
    compiler/c.capnp
    compiler/c.capnp.h
    DESTINATION include/capnp
)