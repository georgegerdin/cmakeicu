cmake_minimum_required(VERSION 3.2)
include(ExternalProject)

set(ICU_PREFIX			"${CMAKE_CURRENT_BINARY_DIR}/icu")

	if(MSVC)
		if(CMAKE_BUILD_TYPE MATCHES DEBUG)
			set(CONFIGURATION "Debug")
		else()
			set(CONFIGURATION "Release")
		endif()	
		
		if(CMAKE_SIZEOF_VOID_P EQUAL 8)
			set(PLATFORM "x64")
		else()
			set(PLATFORM "Win32")
		endif()
			
	
		ExternalProject_add(icu
			PREFIX ${ICU_PREFIX}
			URL "http://download.icu-project.org/files/icu4c/57.1/icu4c-57_1-src.zip"
			URL_MD5 "f797503ecaebf1d38920013dc7893066"
			CONFIGURE_COMMAND devenv <BINARY_DIR>/source/allinone/allinone.sln /upgrade
			BUILD_COMMAND msbuild <BINARY_DIR>/source/allinone/allinone.sln /p:Configuration=${CONFIGURATION} /p:Platform=${PLATFORM}
			INSTALL_COMMAND rem nop
			BUILD_IN_SOURCE 1
		)			
	else()
	
	endif()