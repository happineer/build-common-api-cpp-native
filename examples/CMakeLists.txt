cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread -std=c++0x")

# Include symbols, just in case we need to debug a crash
set(CMAKE_BUILD_TYPE Debug)

include_directories(
    src-gen
    ../capicxx-core-runtime/include
    ../capicxx-someip-runtime/include
    ../vsomeip/interface
)

# CMake needs the policy set to avoid warning about
# how to handle a relative path for linking.
# Then we set link search path relative to the source
# dir (thus reaching the locally built libraries we created)
# - Gunnar
cmake_policy(SET CMP0015 NEW)
link_directories(
    ../capicxx-core-runtime/build
    ../capicxx-someip-runtime/build
    ../vsomeip/build
)

add_executable(HelloWorldClient
    src/HelloWorldClient.cpp
)
target_link_libraries(HelloWorldClient CommonAPI)

add_executable(HelloWorldService
    src/HelloWorldService.cpp
    src/HelloWorldStubImpl.cpp
)
target_link_libraries(HelloWorldService CommonAPI)

#SOMEIP examples
add_executable(HelloWorldSomeIPClient
	src/HelloWorldClient.cpp
	src-gen/v1/commonapi/HelloWorldSomeIPProxy.cpp
	src-gen/v1/commonapi/HelloWorldSomeIPDeployment.cpp
)
target_link_libraries(HelloWorldSomeIPClient CommonAPI CommonAPI-SomeIP vsomeip3)
add_executable(HelloWorldSomeIPService
	src/HelloWorldService.cpp
	src/HelloWorldStubImpl.cpp
	src-gen/v1/commonapi/HelloWorldSomeIPStubAdapter.cpp
	src-gen/v1/commonapi/HelloWorldSomeIPDeployment.cpp
)
target_link_libraries(HelloWorldSomeIPService CommonAPI CommonAPI-SomeIP vsomeip3)
