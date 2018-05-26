# 요구 CMake 최소 버전
CMAKE_MINIMUM_REQUIRED ( VERSION 2.8 )

 
# 프로젝트 이름 및 버전
PROJECT ( FOO )
SET ( PROJECT_VERSION_MAJOR 0 )
SET ( PROJECT_VERSION_MINOR 1 )
 
# 빌드 형상(Configuration) 및 주절주절 Makefile 생성 여부
SET ( CMAKE_BUILD_TYPE Release )
SET ( CMAKE_VERBOSE_MAKEFILE true )
 
# 빌드 대상 바이너리 파일명 및 소스 파일 목록
SET ( OUTPUT_ELF
        "${CMAKE_PROJECT_NAME}-${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.out"
        )
SET ( SRC_FILES 
    main.cpp

    )


# 공통 컴파일러
#SET ( CMAKE_C_COMPILER "arm-hisiv400-linux-gcc" )
#SET ( CMAKE_CXX_COMPILER "arm-hisiv400-linux-g++" ) 
SET(CMAKE_STRIP "strip")
 



# 공통 컴파일 옵션, 링크 옵션
ADD_DEFINITIONS ( 

    )
ADD_COMPILE_OPTIONS ( 
    -g -Wall -fno-aggressive-loop-optimizations -Wall -fpermissive -fexceptions)

SET ( CMAKE_EXE_LINKER_FLAGS " ")

#execute_process(COMMAND pkg-config --cflags alsa freetype2 libcurl x11 xext xinerama webkit2gtk-4.0 gtk+-x11-3.0 OUTPUT_VARIABLE FOO)
find_package(PkgConfig)
pkg_check_modules(FREETYPE freetype2)
pkg_check_modules(GTK gtk+-x11-3.0)
pkg_check_modules(ALSA alsa)
pkg_check_modules(LIBCURL libcurl)
pkg_check_modules(X11 x11)
pkg_check_modules(XEXT xext)
pkg_check_modules(XINERAMA xinerama)
pkg_check_modules(WEBKIT2GTK webkit2gtk-4.0)
pkg_check_modules(OPENGL gl)
pkg_check_modules(XF86 xxf86vm)
pkg_check_modules(XRANDR xrandr)
pkg_check_modules(XCURSOR xcursor)
pkg_check_modules(XINERAMA xinerama)





 
# 공통 링크 라이브러리 (-l)
LINK_LIBRARIES( -Wl,-Bstatic nanogui -Wl,-Bdynamic pthread dl
    ) 
 
# 공통 링크 라이브러리 디렉토리 (-L)
LINK_DIRECTORIES ( 
    ${FREETYPE_LIBRARY_DIRS} 
    ${GTK_LIBRARY_DIRS} 
    ${ALSA_LIBRARY_DIRS}
    ${LIBCURL_LIBRARY_DIRS}
    ${X11_LIBRARY_DIRS}
    ${XEXT_LIBRARY_DIRS}
    ${XINERAMA_LIBRARY_DIRS}
    ${WEBKIT2GTK_LIBRARY_DIRS}
    ${OPENGL_LIBRARY_DIRS}
    ./pcnano/lib
    )
#TARGET_LINK_LIBRARIES(${OUTPUT_ELF} Bdynamic pthread )

# 공통 헤더 파일 Include 디렉토리 (-I)
INCLUDE_DIRECTORIES ( 
    ${FREETYPE_INCLUDE_DIRS} 
    ${GTK_INCLUDE_DIRS}
    ${ALSA_INCLUDE_DIRS}
    ${LIBCURL_INCLUDE_DIRS}
    ${X11_INCLUDE_DIRS}
    ${XEXT_INCLUDE_DIRS}
    ${XINERAMA_INCLUDE_DIRS}
    ${WEBKIT2GTK_INCLUDE_DIRS}
    ${OPENGL_INCLUDE_DIRS}
    ./pcnano/include
    ./ext/eigen/Eigen
    ./ext/eigen
    ./ext/glfw/include
    ./ext/nanovg/src
    
    )
 
# "Debug" 형상 한정 컴파일 옵션, 링크 옵션
SET ( CMAKE_C_FLAGS_DEBUG "-DDEBUG -DC_FLAGS" )
SET ( CMAKE_EXE_LINKER_FLAGS_DEBUG "-DDEBUG -DLINKER_FLAGS" )

SET ( CMAKE_CXX_FLAGS "-std=c++14 -w")
 
# "Release" 형상 한정 컴파일 옵션, 링크 옵션
SET ( CMAKE_C_FLAGS_RELEASE "" )
SET ( CMAKE_EXE_LINKER_FLAGS_RELEASE "-DRELEASE -DLINKER_FLAGS " )
 
# 출력 디렉토리
SET ( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BUILD_TYPE} )
SET ( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BUILD_TYPE}/lib )
SET ( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BUILD_TYPE}/lib )
 
# 빌드 대상 바이너리 추가
ADD_EXECUTABLE( ${OUTPUT_ELF} ${SRC_FILES} )
TARGET_LINK_LIBRARIES(${OUTPUT_ELF}
    ${FREETYPE_LIBRARIES}
    ${GTK_LIBRARIES}
    ${ALSA_LIBRARIES}
    ${LIBCURL_LIBRARIES}
    ${X11_LIBRARIES}
    ${XEXT_LIBRARIES}
    ${WEBKIT2GTK_LIBRARIES}
    ${XF86_LIBRARIES}
    ${OPENGL_LIBRARIES}
    ${XRANDR_LIBRARIES}
    ${XCURSOR_LIBRARIES}
    ${XINERAMA_LIBRARIES}
    )


ADD_CUSTOM_COMMAND (
    TARGET ${OUTPUT_ELF}
    POST_BUILD
    COMMAND ${CMAKE_STRIP} Release/${OUTPUT_ELF}
    )
