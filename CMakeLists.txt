# LUA

cmake_minimum_required(VERSION 2.8) 
project(Lua)
include(ExternalProject)


set(
	LUA_UTIL
	lua_util.c
	lua_util.h
	)

include_directories(luajit/src)

ExternalProject_Add(
	Luajit
	#URL http://luajit.org/download/LuaJIT-2.0.1.tar.gz
	#PREFIX ${CMAKE_CURRENT_SOURCE_DIR}/lua
	SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/luajit
	#SOURCE_DIR ${CMAKEluajit/src
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
set(libluajit_path ${CMAKE_CURRENT_SOURCE_DIR}/lua/src/Luajit/src)
include_directories(${libluajit_path})
message("Libluajit: " ${libluajit_path})
set_target_properties(libluajit PROPERTIES IMPORTED_LOCATION ${libluajit_path}/libluajit.a)
add_dependencies(libluajit Luajit)

