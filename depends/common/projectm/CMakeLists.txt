cmake_minimum_required(VERSION 3.5)
project(projectm)

if (APP_RENDER_SYSTEM STREQUAL "gles")
  set(PROJECTM_CONFIG "--enable-gles")
endif()

set(LDFLAG_build "")

list(APPEND LDFLAG_build -L${OUTPUT_DIR}/lib
                         -Wl,-framework,OpenGLES
                         -L${CMAKE_CURRENT_SOURCE_DIR}/lib)

include(ExternalProject)
externalproject_add(projectm
                    SOURCE_DIR ${CMAKE_SOURCE_DIR}
                    UPDATE_COMMAND autoreconf -vif
                    CONFIGURE_COMMAND  bash --login -c "./configure \
                      LDFLAGS=${LDFLAG_build} \
                      CFLAGS='-I${OUTPUT_DIR}/include -I${CMAKE_CURRENT_SOURCE_DIR}/include ${CMAKE_C_FLAGS}' \
                      CXXFLAGS='-I${OUTPUT_DIR}/include -I${CMAKE_CURRENT_SOURCE_DIR}/include ${CMAKE_C_FLAGS}' \
                      --disable-shared \
                      --disable-qt \
                      --enable-preset-subdirs \
                      --prefix=${OUTPUT_DIR} \
                      --enable-gles \
                      --with-pic ${PROJECTM_CONFIG}"}
                    INSTALL_COMMAND ""
                    BUILD_IN_SOURCE 1)

install(CODE "execute_process(COMMAND make install WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})")
