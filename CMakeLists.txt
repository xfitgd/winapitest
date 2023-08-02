cmake_minimum_required(VERSION 3.26)


set(CMAKE_EXPORT_COMPILE_COMMANDS ON)#VSCODE로 설정 내보내기

project(winapitest LANGUAGES C)#C++면 CXX

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
add_compile_options(-fno-limit-debug-info -std=c2x)# 디버그 정보 보존, c23
add_compile_definitions(DEBUG _DEBUG)#매크로 추가
else()
add_compile_definitions(NDEBUG _NDEBUG)
add_compile_options(-O3 -std=c2x)# 최대 최적화
endif()

#include_directories() 외부 헤더 포함 경로 추가

# WIN32없으면 위 주석 subsystem 명시 필요
add_executable(winapitest WIN32 main.c)


# 외부 라이브러리 링크 실행파일 만들때는 PUBLIC PRIVATE 생략가능
#target_link_libraries(winapitest)