
add_library(tas_original_sockets SHARED 
        sockets/control.c
        sockets/transfer.c
        sockets/context.c
        sockets/manage_fd.c
        sockets/epoll.c
        sockets/poll.c
        sockets/libc.c
        tas/init.c
        tas/kernel.c
        tas/conn.c
        tas/connect.c
        utils/rng.c
        utils/timeout.c
        utils/utils.c
    )

add_library(tas_original_interpose SHARED
        sockets/control.c
        sockets/transfer.c
        sockets/context.c
        sockets/manage_fd.c
        sockets/epoll.c
        sockets/poll.c
        sockets/libc.c
        sockets/interpose.c
        tas/init.c
        tas/kernel.c
        tas/conn.c
        tas/connect.c
        utils/rng.c
        utils/timeout.c
        utils/utils.c
    )

include_directories(
        sockets/include/
        tas/include/
    )

target_link_libraries(tas_original_sockets rt dl)
target_link_libraries(tas_original_interpose rt dl)
