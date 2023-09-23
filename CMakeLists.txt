cmake_minimum_required(VERSION 3.26)#3 이상 버전 권장

# set(CMAKE_GENERATOR "Ninja") # 빌드 툴 설정
# set(CMAKE_C_COMPILER clang) # 스크립트 내에서 컴파일러 경로 설정
# set(CMAKE_CXX_COMPILER clang++)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)#VSCODE로 설정 내보내기

project(winapitest LANGUAGES C)#C++면 CXX

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(MSVC) # MSVC(Visual Studio) Compiler
        add_compile_options("/std:c17") # c17
        set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreadedDebugDLL") # 실행시 별도의 CRT dll 필요, 정적 빌드 시 DLL 제거
    else() # gcc or clang
        add_compile_options(-fno-limit-debug-info -std=c2x)# 디버그 정보 보존, c23(버전 설정 예 필수 아님)
        # -static 옵션은 libc++/libstdc++ 포함시 적용됨 -> libc++.dll(so)을 프로그램 실행시 요구하지 않음(정적 빌드)
    endif()
    add_compile_definitions(DEBUG _DEBUG)#매크로 추가
else()
    if(MSVC)
        add_compile_options("/O2 /std:c17")
        set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreadedDLL")
    else()
        add_compile_options(-O3 -std=c2x)# 최대 최적화
    endif()
    add_compile_definitions(NDEBUG _NDEBUG)
endif()

#include_directories() 외부 헤더 경로 추가
#link_directories() 외부 라이브러리 경로 추가

# WIN32없으면 밑 주석 -Wl,--subsystem,windows 명시 필요
add_executable(winapitest WIN32 main.c)

#target_link_options(winapitest PUBLIC -Wl,--subsystem,windows) # msvc는 /SUBSYSTEM:WINDOWS

# 외부 라이브러리 링크 (실행파일 만들때는 PUBLIC PRIVATE 생략가능)
#target_link_libraries(winapitest zlib example/lib/exlib.a) # zlib는 설정해둔 경로에 있는 libzlib.a 추가(컴파일러 설정에 따라 다름), exlib.a는 전체경로 예시
