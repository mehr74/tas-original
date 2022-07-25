cmake_minimum_required(VERSION 3.14)
project(tas)

include_directories(include)

SET(CMAKE_C_FLAGS "-std=gnu99 -g -Wall -march=native \
                -fno-omit-frame-pointer")
SET(DPDK_LIBS1 "-Wl,--whole-archive")
SET(DPDK_LIBS2 "-lm -pthread -lrt -lnuma \
                -Wl,--no-whole-archive")

add_executable(tas_original
    tas/tas.c
    tas/config.c
    tas/blocking.c
    tas/slow/kernel.c
    tas/slow/packetmem.c
    tas/slow/appif.c
    tas/slow/appif_ctx.c
    tas/slow/nicif.c
    tas/slow/cc.c
    tas/slow/tcp.c
    tas/slow/arp.c
    tas/slow/routing.c
    tas/slow/kni.c
    tas/fast/fastemu.c
    tas/fast/fastemu.h
    tas/fast/network.c
    tas/fast/network.h
    tas/fast/qman.c
    tas/fast/trace.c
    tas/fast/fast_kernel.c
    tas/fast/fast_appctx.c
    tas/fast/fast_flows.c
    lib/utils/rng.c
    lib/utils/timeout.c
    lib/utils/utils.c
    )

target_link_directories(tas_original PRIVATE $ENV{RTE_SDK}/lib)

include_directories(tas_original
    include
    tas/include
    tas/include/tas/
    $ENV{RTE_SDK}/include
    $ENV{RTE_SDK}/include/dpdk
    $ENV{RTE_SDK}/include/x86_64-linux-gnu/dpdk)

target_link_libraries(tas_original
    ${DPDK_LIBS1}
    rte_eal 
    rte_mempool
    rte_mempool_ring
    rte_hash
    rte_ring
    rte_kvargs
    rte_ethdev
    rte_mbuf
    numa
    rte_bus_pci
    rte_pci
    rte_cmdline
    rte_timer
    rte_net
    rte_kni
    rte_bus_vdev
    rte_gso
    ${DPDK_LIBS2}
    dl
    )

target_include_directories(tas_original PRIVATE include)
target_include_directories(tas_original PRIVATE tas/include)

add_subdirectory(lib)
