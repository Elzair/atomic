cmake_minimum_required(VERSION 3.3)

project(boost_atomic)

option(BOOST_ATOMIC_SHARED_LIB "Enable dynamic linking" ON)

set(SOURCE_FILES src/lockpool.cpp)

if(BOOST_ATOMIC_SHARED_LIB)
  add_library(boost_atomic SHARED ${SOURCE_FILES} )
  target_compile_definitions(boost_atomic PRIVATE BOOST_ATOMIC_DYN_LINK=1)
else()
  add_library(boost_atomic STATIC ${SOURCE_FILES})
  target_compile_definitions(boost_atomic PRIVATE BOOST_ATOMIC_STATIC_LINK=1)
endif(BOOST_ATOMIC_SHARED_LIB)

target_include_directories(boost_atomic BEFORE PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(boost_atomic boost_predef boost_config boost_assert boost_core)

install(TARGETS boost_atomic DESTINATION lib)
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/boost DESTINATION include)

add_library(boost::atomic ALIAS boost_atomic)
