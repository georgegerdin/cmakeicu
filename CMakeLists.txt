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
	
		if(CMAKE_BUILD_TYPE MATCHES DEBUG)
			set(BUILD_TYPE_STRING "--enable-debug --disable-release --with-library-suffix=d")
		else()
			set(BUILD_TYPE_STRING "--disable-debug --enable-release")
		endif()
		
		if(BUILD_SHARED_LIBS)
			set(LINK_TYPE_STRING "--disable-static --enable-shared")
		else()
			set(LINK_TYPE_STRING "--enable-static --disable-shared")
		endif()
		
		if(APPLE)
			set(PLATFORM "MacOSX")
			set(MAKE_COMMAND "gnumake")
		else()
			set(MAKE_COMMAND "make")
		endif()

		if(MINGW)
			set(PLATFORM "MinGW")
		elseif(UNIX)
			set(PLATFORM "Linux")
		endif()
		
		ExternalProject_add(icu
			PREFIX ${ICU_PREFIX}
			URL "http://download.icu-project.org/files/icu4c/57.1/icu4c-57_1-src.zip"
			URL_MD5 "f797503ecaebf1d38920013dc7893066"
			#PATCH_COMMAND ${CMAKE_COMMAND} -E copy_directory <BINARY_DIR>/source <BINARY_DIR>
			#PATCH_COMMAND ${CMAKE_COMMAND} -E copy <BINARY_DIR>/LICENSE <BINARY_DIR>/../
			CONFIGURE_COMMAND cd <BINARY_DIR>/source && ./runConfigureICU  ${PLATFORM} --prefix=${CMAKE_CURRENT_BINARY_DIR} ${BUILD_TYPE_STRING} ${LINK_TYPE_STRING}
			BUILD_COMMAND cd <BINARY_DIR>/source && make
			INSTALL_COMMAND cd <BINARY_DIR>/source && make install 
			BUILD_IN_SOURCE 1
		)
	
	endif()
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	