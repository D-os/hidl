add_library(hidl-gen-utils SHARED
  "FQName.cpp"
  "Formatter.cpp"
  "FqInstance.cpp"
  "StringHelper.cpp"
)
target_include_directories(hidl-gen-utils PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/hidl-util>
)
target_link_libraries(hidl-gen-utils base)
