cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
project(demo LANGUAGES CXX)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(FairCMakeModules REQUIRED)
include(FairFindPackage2)
find_package2(PRIVATE FairMQ REQUIRED COMPONENTS tidy_tool)
find_package2_implicit_dependencies()

include(FairMQTidy)
set(sampler "${PROJECT_NAME}-sampler")
add_executable(${sampler} sampler.cxx)
target_link_libraries(${sampler} PRIVATE FairMQ::FairMQ)
fairmq_target_tidy(TARGET ${sampler})

set(sink "${PROJECT_NAME}-sink")
add_executable(${sink} sink.cxx)
target_link_libraries(${sink} PRIVATE FairMQ::FairMQ)
fairmq_target_tidy(TARGET ${sink})

include(GNUInstallDirs)
install(TARGETS ${sampler} ${sink} RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

include(FairSummary)
fair_summary_package_dependencies()
message(STATUS "  ")
