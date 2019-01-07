cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{VITASDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

project(fucku)
include("${VITASDK}/share/vita.cmake" REQUIRED)

set(PROJECT_TITLE "FuckU")
set(TITLE_ID "FUCK44444")
set(VITA_VERSION  "01.00")

set(CMAKE_C_COMPILER "arm-vita-eabi-gcc")
set(CMAKE_CXX_COMPILER "arm-vita-eabi-g++")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu11")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

add_executable(${PROJECT_NAME}
  main.c
  debugScreen.c
  debugScreenFont.c
)

target_link_libraries(${PROJECT_NAME}
  SceDisplay_stub
  SceCtrl_stub
)

vita_create_self(eboot.bin ${PROJECT_NAME}.elf)
vita_create_vpk(${PROJECT_NAME}.vpk ${TITLE_ID} eboot.bin
  VERSION ${VITA_VERSION}
  NAME ${PROJECT_TITLE}
  FILE ${CMAKE_SOURCE_DIR}/sce_sys/icon0.png sce_sys/icon0.png
  FILE ${CMAKE_SOURCE_DIR}/sce_sys/livearea/contents/bg.png sce_sys/livearea/contents/bg0.png
  FILE ${CMAKE_SOURCE_DIR}/sce_sys/livearea/contents/pic0.png sce_sys/pic0.png
  FILE ${CMAKE_SOURCE_DIR}/sce_sys/livearea/contents/template.xml sce_sys/livearea/contents/template.xml
)

