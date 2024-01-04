cmake_minimum_required(VERSION 3.16)

project(products_telegram_bot_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE PACKAGE_TEMPLATE")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(products_telegram_bot_shared_dir "lib")
set(products_telegram_bot_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "products_telegram_bot_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "products_telegram_bot")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "products_telegram_bot")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "products_telegram_bot")
endif()


add_library(products_telegram_bot_shared SHARED products_telegram_bot.cpp)
add_executable(products_telegram_bot_exe products_telegram_bot.cpp)

set_target_properties(products_telegram_bot_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${products_telegram_bot_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(products_telegram_bot_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${products_telegram_bot_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(products_telegram_bot_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET products_telegram_bot_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${products_telegram_bot_shared_dir}/${prefix_shared_library}products_telegram_bot${package_shared_extension}"
)

add_custom_command(
  TARGET products_telegram_bot_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${products_telegram_bot_exe_dir}/products_telegram_bot${package_exe_extension}" 
)

 