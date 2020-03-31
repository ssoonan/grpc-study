cmake_minimum_required(VERSION 3.5)
project(grpc_study)

find_package(Threads REQUIRED)  # 얘를 따로 넣는 이유? 어디에 넣으라고 말은 없는데?

find_package(Protobuf CONFIG REQUIRED)
message(STATUS "Using protobuf ${protobuf_VERSION}")
find_program(_PROTOBUF_PROTOC protoc)

find_package(gRPC CONFIG REQUIRED)
message(STATUS "Using gRPC ${gRPC_VERSION}")
find_program(_GRPC_CPP_PLUGIN_EXECUTABLE grpc_cpp_plugin)

set(_PROTOBUF_LIBPROTOBUF protobuf::libprotobuf)
set(_GRPC_GRPCPP_UNSECURE gRPC::grpc++_unsecure)  # 설치한 protobuf, grpc의 shared_library를 link하기 위한 변수
# 라이브러리 규모가 크면, 그 안에서도 특정 shared_library만 가져와 link 할 수도 있다
# TODO: 그 라이브러리에서 어떤 걸 가져와야 link 해야하지의 여부는 어떻게 판단?

add_executable(greet_server greet_server.cpp helloworld.grpc.pb.cc helloworld.pb.cc)
target_link_libraries(greet_server ${_GRPC_GRPCPP_UNSECURE} ${_PROTOBUF_LIBPROTOBUF})