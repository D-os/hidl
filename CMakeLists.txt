include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

add_library(hidl-gen-hash SHARED
    "Hash.cpp"
)
target_include_directories(hidl-gen-hash PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include_hash/hidl-hash>
    $<INSTALL_INTERFACE:include_hash>
)
target_link_libraries(hidl-gen-hash base crypto ssl)

add_subdirectory(utils)

add_library(hidl-gen SHARED
  "Annotation.cpp"
  "ArrayType.cpp"
  "CompoundType.cpp"
  "ConstantExpression.cpp"
  "DeathRecipientType.cpp"
  "DocComment.cpp"
  "EnumType.cpp"
  "HandleType.cpp"
  "HidlTypeAssertion.cpp"
  "Interface.cpp"
  "Location.cpp"
  "MemoryType.cpp"
  "Method.cpp"
  "NamedType.cpp"
  "PointerType.cpp"
  "FmqType.cpp"
  "RefType.cpp"
  "ScalarType.cpp"
  "Scope.cpp"
  "StringType.cpp"
  "Type.cpp"
  "TypeDef.cpp"
  "VectorType.cpp"
)
target_include_directories(hidl-gen PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include_hash>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/utils/include>
)
target_link_libraries(hidl-gen base hidl-gen-hash hidl-gen-utils)

find_package(BISON)
find_package(FLEX)

BISON_TARGET(hidl-gen_y
    "hidl-gen_y.yy" ${CMAKE_CURRENT_BINARY_DIR}/hidl-gen_y.cpp)
FLEX_TARGET(hidl-gen_l
    "hidl-gen_l.ll" ${CMAKE_CURRENT_BINARY_DIR}/hidl-gen_l.cpp)
ADD_FLEX_BISON_DEPENDENCY(hidl-gen_l hidl-gen_y)

add_library(hidl-gen-ast SHARED
  "Coordinator.cpp"
  "generateCpp.cpp"
  "generateCppAdapter.cpp"
  "generateCppImpl.cpp"
  "generateJava.cpp"
  "generateVts.cpp"
  "hidl-gen_y.yy"
  "hidl-gen_y.cpp"
  "hidl-gen_l.ll"
  "hidl-gen_l.cpp"
  "AST.cpp"
)
target_link_libraries(hidl-gen-ast base hidl-gen hidl-gen-hash hidl-gen-utils)

add_executable(hidl-gen-bin
  "main.cpp"
)
target_link_libraries(hidl-gen-bin base hidl-gen hidl-gen-ast hidl-gen-hash hidl-gen-utils)
set_target_properties(hidl-gen-bin
  PROPERTIES OUTPUT_NAME hidl-gen)
