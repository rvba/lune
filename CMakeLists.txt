# LUA

GET_PROPERTY(_LUA_LIB GLOBAL PROPERTY LUA_LIB)

if( NOT DEFINED _LUA_LIB)
	SET_PROPERTY(GLOBAL PROPERTY LUA_LIB "not set")
	set(_LUA_LIB "not set")
endif() 


if( NOT ${_LUA_LIB} STREQUAL "set")
	SET_PROPERTY(GLOBAL PROPERTY LUA_LIB "set")

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

if(OPT_LUAJIT_PACKAGE)
else()

ExternalProject_Add(
	Luajit
	SOURCE_DIR ${luajit_src}
	UPDATE_COMMAND ""
	CONFIGURE_COMMAND ""
	BUILD_COMMAND ${MAKE} 
	BUILD_IN_SOURCE 1
	INSTALL_COMMAND ""
	LOG_CONFIGURE 1
	LOG_BUILD 1
	)

add_library(libluajit STATIC IMPORTED)
include_directories(${luajit_src}/src)

set_target_properties(libluajit PROPERTIES IMPORTED_LOCATION ${luajit_src}/src/libluajit.a)
add_dependencies(libluajit Luajit)
add_dependencies(lua_util Luajit)
endif()

endif()

