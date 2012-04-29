if(DEFINED ALONE_BUILD)
  include_directories(
	${PCAP_DIR}/Include
	${NET_DIR}/lwip/win32/include
	)
endif()

set(LWIP_DIR ${NET_DIR}/lwip/src)
set(LWIP_API
	${LWIP_DIR}/api/api_lib.c
	${LWIP_DIR}/api/api_msg.c
	${LWIP_DIR}/api/err.c
	${LWIP_DIR}/api/netbuf.c
	${LWIP_DIR}/api/netdb.c
	${LWIP_DIR}/api/netifapi.c
	${LWIP_DIR}/api/sockets.c
	${LWIP_DIR}/api/tcpip.c
)
set(LWIP_IP4
	${LWIP_DIR}/core/ipv4/ip4.c
	${LWIP_DIR}/core/ipv4/ip4_addr.c
	${LWIP_DIR}/core/ipv4/ip_frag.c
)
set(LWIP_CORE
	${LWIP_DIR}/core/def.c
	${LWIP_DIR}/core/dns.c
	${LWIP_DIR}/core/inet_chksum.c
	${LWIP_DIR}/core/init.c
	${LWIP_DIR}/core/mem.c
	${LWIP_DIR}/core/memp.c
	${LWIP_DIR}/core/netif.c
	${LWIP_DIR}/core/pbuf.c
	${LWIP_DIR}/core/raw.c
	${LWIP_DIR}/core/stats.c
	${LWIP_DIR}/core/sys.c
	${LWIP_DIR}/core/tcp.c
	${LWIP_DIR}/core/tcp_in.c
	${LWIP_DIR}/core/tcp_out.c
	${LWIP_DIR}/core/timers.c
	${LWIP_DIR}/core/udp.c
)
set(LWIP_NETIF
	${LWIP_DIR}/core/dhcp.c
	${LWIP_DIR}/netif/etharp.c
	${LWIP_DIR}/netif/ethernetif.c

	${NET_DIR}/lwip/win32/sys_arch.c
	${NET_DIR}/lwip/win32/pcapif.c
	${NET_DIR}/lwip/win32/pcapif_helper.c
)
set(LWIP_PPP
	${LWIP_DIR}/netif/ppp/auth.c
	${LWIP_DIR}/netif/ppp/chap.c
	${LWIP_DIR}/netif/ppp/chpms.c
	${LWIP_DIR}/netif/ppp/fsm.c
	${LWIP_DIR}/netif/ppp/ipcp.c
	${LWIP_DIR}/netif/ppp/lcp.c
	${LWIP_DIR}/netif/ppp/magic.c
	${LWIP_DIR}/netif/ppp/md5.c
	${LWIP_DIR}/netif/ppp/pap.c
	${LWIP_DIR}/netif/ppp/ppp.c
	${LWIP_DIR}/netif/ppp/randm.c
	${LWIP_DIR}/netif/ppp/vj.c
)

if(DEFINED ALONE_BUILD)
  set(LWIP_SRCS
	${LWIP_CORE}
	${LWIP_API}
	${LWIP_IP4}
	${LWIP_NETIF}
	)  
else()
  set(LWIP_SRCS
	${LWIP_CORE}
	${LWIP_API}
	${LWIP_IP4}
	${LWIP_PPP}

	${NET_DIR}/lwip_sys_arch.cpp
	)
endif()

SOURCE_GROUP(core FILES ${LWIP_CORE})
SOURCE_GROUP(api FILES ${LWIP_API})
SOURCE_GROUP(ip4 FILES ${LWIP_IP4})
SOURCE_GROUP(netif FILES ${LWIP_NETIF})

if(DEFINED EPOC_EXTRA_FLAGS)
set_source_files_properties(${LWIP_SRCS}, PROPERTIES COMPILE_FLAGS ${EPOC_EXTRA_FLAGS})
endif()

add_library(
	lwip STATIC
	${LWIP_SRCS}
)

