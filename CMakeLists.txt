cmake_minimum_required(VERSION 3.19)

set(CMAKE_TOOLCHAIN_FILE "$ENV{SCE_PSP2_SDK_DIR}/host_tools/build/cmake/psp2-snc-toolchain.cmake")

project(pib LANGUAGES C)

include(VitaDevelopmentSuite)
set(CMAKE_C_STANDARD 99)

add_compile_options(
  -Xdiag=0 -Xquit=2 -O3
)

add_link_options(
  -s
)

include_directories(
  ${VDSUITE_USER_INCLUDE_DIRECTORIES}
  ${VDSUITE_STANDARD_INCLUDE_DIRECTORIES}
  include
)

link_directories(
  ${VDSUITE_LIBRARY_DIRECTORIES}
  ${CMAKE_CURRENT_BINARY_DIR}
)

add_library(${PROJECT_NAME} STATIC
  src/pib.c
  src/hooks.c
  src/patches.c
  src/sha1.c
  src/shacccgpatch.c
  src/sysmodepatch.c
)

add_definitions(-DPIB_PRX)

add_library("${PROJECT_NAME}.suprx" SHARED
  src/pib.c
  src/hooks.c
  src/patches.c
  src/sha1.c
  src/shacccgpatch.c
  src/sysmodepatch.c
)

target_link_libraries("${PROJECT_NAME}.suprx"
  SceGxm_stub_weak
  SceDisplay_stub_weak
  SceThreadmgr_stub_weak
  SceLibKernel_stub_weak
  libScePiglet_stub_weak
  taihen_stub_weak
  SceShaccCg_stub_weak
  SceAppMgr_stub_weak
  SceSharedFb_stub_weak
  SceGxmInternalForVsh_stub_weak
  SceGxmInternal_stub_weak
)