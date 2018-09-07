set(LLVM_LINK_COMPONENTS support)

set(Boost_USE_STATIC_LIBS ON)
find_package(Boost 1.67.0 REQUIRED system filesystem)
if(Boost_FOUND)
   include_directories (SYSTEM ${Boost_INCLUDE_DIR})
endif()

include_directories(include)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/agrio-cc.cpp.in ${CMAKE_BINARY_DIR}/agrio-cc.cpp)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/agrio-cpp.cpp.in ${CMAKE_BINARY_DIR}/agrio-cpp.cpp)

add_clang_executable(agrio-cc ${CMAKE_BINARY_DIR}/agrio-cc.cpp ${CMAKE_CURRENT_SOURCE_DIR}/../whereami/whereami.cpp)
target_compile_options(agrio-cc PRIVATE -fexceptions)
target_include_directories(agrio-cc PUBLIC ${Boost_INCLUDE_DIR} ${CMAKE_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../include ${CMAKE_CURRENT_SOURCE_DIR}/../whereami)
target_link_libraries(agrio-cc PRIVATE
  clangTooling
  clangBasic
  clangASTMatchers
  ${Boost_LIBRARIES})

add_clang_executable(agrio-cpp ${CMAKE_BINARY_DIR}/agrio-cpp.cpp ${CMAKE_CURRENT_SOURCE_DIR}/../whereami/whereami.cpp)
target_compile_options(agrio-cpp PRIVATE -fexceptions)
target_include_directories(agrio-cpp PUBLIC ${Boost_INCLUDE_DIR} ${CMAKE_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../include ${CMAKE_CURRENT_SOURCE_DIR}/../whereami)
target_link_libraries(agrio-cpp PRIVATE
  clangTooling
  clangBasic
  clangASTMatchers
  ${Boost_LIBRARIES})

install(FILES ${CMAKE_BINARY_DIR}/bin/agrio-cpp DESTINATION ${CMAKE_INSTALL_BINDIR} PERMISSIONS OWNER_READ OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)
install(FILES ${CMAKE_BINARY_DIR}/bin/agrio-cc DESTINATION ${CMAKE_INSTALL_BINDIR} PERMISSIONS OWNER_READ OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)
