cmake_minimum_required(VERSION 3.5)
project(projectm)

if (APP_RENDER_SYSTEM STREQUAL "gles")
  set(PROJECTM_CONFIG "--enable-gles")
endif()

set(LDFLAG_build "")

list(APPEND LDFLAG_build -L${OUTPUT_DIR}/lib
                         -Wl,-framework,OpenGLES)

include(ExternalProject)
externalproject_add(projectm
                    SOURCE_DIR ${CMAKE_SOURCE_DIR}
                    UPDATE_COMMAND autoreconf -vif
                    CONFIGURE_COMMAND  ./configure
                      LDFLAGS=${LDFLAG_build}
                      CFLAGS='${CMAKE_C_FLAGS}' \
                      CXXFLAGS='${CMAKE_C_FLAGS}' \
                      --disable-shared
                      --disable-qt
                      --enable-preset-subdirs
                      --prefix=${OUTPUT_DIR}
                      --enable-gles
                      --with-pic ${PROJECTM_CONFIG}
                    INSTALL_COMMAND ""
                    BUILD_IN_SOURCE 1)

install(CODE "execute_process(COMMAND make install WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})")
