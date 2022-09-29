include_directories(${CMAKE_SOURCE_DIR})

set(srcs
  compressed-matrix.cc
  io-funcs.cc
  kaldi-holder.cc
  kaldi-io.cc
  kaldi-matrix.cc
  kaldi-semaphore.cc
  kaldi-table.cc
  kaldi-utils.cc
  kaldi-vector.cc
  matrix-shape.cc
  parse-options.cc
  posterior.cc
  text-utils.cc
  wave-reader.cc

  fbank/feature-fbank.cc
  fbank/feature-functions.cc
  fbank/feature-window.cc
  fbank/fftsg.c
  fbank/log.cc
  fbank/mel-computations.cc
  fbank/online-feature.cc
  fbank/rfft.cc
)

add_library(kaldiio STATIC ${srcs})
if(APPLE)
  set_target_properties(kaldiio
    PROPERTIES
      LINK_FLAGS "-undefined dynamic_lookup"
  )
endif()

file(MAKE_DIRECTORY
  DESTINATION
    ${PROJECT_BINARY_DIR}/include/kaldiio
)

file(GLOB all_headers *.h)
file(GLOB all_headers_fbank fbank/*.h)

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  message(STATUS "All headers: ${all_headers}")
endif()

file(COPY
  ${all_headers}
  DESTINATION
    ${PROJECT_BINARY_DIR}/include/kaldiio/
)

file(COPY
  ${all_headers_fbank}
  DESTINATION
    ${PROJECT_BINARY_DIR}/include/kaldiio/fbank
)

install(TARGETS kaldiio
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(FILES ${all_headers}
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/kaldiio/fbank
)
