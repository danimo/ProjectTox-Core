cmake_minimum_required(VERSION 2.6.0)
project(toxcore C)

set(core_sources
	DHT.c
	friend_requests.c
	group_chats.c
	LAN_discovery.c
	Lossless_UDP.c
	Messenger.c
	net_crypto.c
	network.c
	ping.c
	tox.c
	util.c
	)

set(core_headers
	DHT.h
	friend_requests.h
	group_chats.h
	LAN_discovery.h
	Lossless_UDP.h
	Messenger.h
	misc_tools.h
	net_crypto.h
	network.h
	ping.h
	tox.h
	util.h
	)

add_library(toxcore SHARED ${core_sources})
add_library(toxcore_static ${core_sources})
set_target_properties(toxcore_static PROPERTIES OUTPUT_NAME toxcore)

target_link_libraries(toxcore ${LINK_CRYPTO_LIBRARY})

install(TARGETS toxcore toxcore_static DESTINATION lib)
install(FILES ${core_headers} DESTINATION include)

if(WIN32)
	target_link_libraries(toxcore ws2_32)
endif()

execute_process(COMMAND git rev-list HEAD --count OUTPUT_VARIABLE COMMIT)

# Write pkgconfig-file:
include(InstallPkgConfigFile)
install_pkg_config_file(toxcore CFLAGS LIBS -ltoxcore REQUIRES VERSION 0.1.1_r${COMMIT})
 
