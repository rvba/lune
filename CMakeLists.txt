# LUA

GET_PROPERTY(_LUA_LIB GLOBAL PROPERTY LUA_LIB)

if( NOT DEFINED _LUA_LIB)
	SET_PROPERTY(GLOBAL PROPERTY LUA_LIB "not set")
	set(_LUA_LIB "not set")
endif() 


if( NOT ${_LUA_LIB} STREQUAL "set")
	SET_PROPERTY(GLOBAL PROPERTY LUA_LIB "set")

message("Lua!!!")
cmake_minimum_required(VERSION 2.8) 
project(Lua)
include(ExternalProject)


set(
	LUA_UTIL
	lua_util.c
	lua_util.h
	)

add_library(lua_util STATIC ${LUA_UTIL})

include_directories(luajit/src)
set(luajit_src ${CMAKE_CURRENT_LIST_DIR}/luajit)
message("luajit_src:" ${luajit_src})

ExternalProject_Add(
	Luajit
	#URL http://luajit.org/download/LuaJIT-2.0.1.tar.gz
	#PREFIX ${CMAKE_CURRENT_SOURCE_DIR}/luajit
	#SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/luajit
	SOURCE_DIR ${luajit_src}
	UPDATE_COMMAND ""
	CONFIGURE_COMMAND ""
	BUILD_COMMAND ${MAKE} 
	BUILD_IN_SOURCE 1
	INSTALL_COMMAND ""
	LOG_CONFIGURE 1
	LOG_BUILD 1
	)

#ExternalProject_Get_Property(luajit install_dir)
add_library(libluajit STATIC IMPORTED)
#set(libluajit_path ${CMAKE_CURRENT_SOURCE_DIR}/src)
#include_directories(${libluajit_path})
#message("Libluajit: " ${libluajit_path})

include_directories(${luajit_src}/src)

#set_target_properties(libluajit PROPERTIES IMPORTED_LOCATION ${libluajit_path}/libluajit.a)
set_target_properties(libluajit PROPERTIES IMPORTED_LOCATION ${luajit_src}/src/libluajit.a)
add_dependencies(libluajit Luajit)
add_dependencies(lua_util Luajit)

endif()

