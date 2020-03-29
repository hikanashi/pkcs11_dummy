
file(GLOB SOURCES
	${CMAKE_CURRENT_SOURCE_DIR}/*.c
)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

add_library(pkcs11 SHARED
	${SOURCES}
)

find_package(OpenSSL REQUIRED)
target_link_libraries(pkcs11 PRIVATE OpenSSL::SSL OpenSSL::Crypto)

message("MSVC:${MSVC}")

if(MSVC)
	set(DLLINSTALLPATH ${CMAKE_SOURCE_DIR}/vcpkg/packages/openssl-windows_x86-windows/lib/engines-1_1)
	file(MAKE_DIRECTORY ${DLLINSTALLPATH})

	add_custom_target(pkcs11install ALL)
	add_custom_command(TARGET pkcs11install POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:pkcs11> ${DLLINSTALLPATH}
		DEPENDS pkcs11)



#	add_custom_command(TARGET pkcs11install POST_BUILD
#		COMMAND ${CMAKE_COMMAND} -E copy_if_different
#		${PROJECT_SOURCE_DIR}/SSL/openssl.cnf
#		${CMAKE_SOURCE_DIR}/vcpkg/packages/openssl-windows_x86-windows)

else()


endif()
