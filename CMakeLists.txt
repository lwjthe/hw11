# 为什么不指定 cmake_minimum_required 会导致下面在 project 处出错？
cmake_minimum_required(VERSION 3.10)

project(hellocmake VERSION 3.1.4 LANGUAGES CXX C)

# 如何让构建类型默认为 Release？
#set(CMAKE_BUILD_TYPE Release)
IF(DEFINED CMAKE_BUILD_TYPE)
  SET(CMAKE_BUILD_TYPE RELEASE)
ENDIF()

# 这样设置 C++14 的方式对吗？请改正
set(CMAKE_CXX_STARDAND 14)

# （可选）使用 ccache 加速编译
find_program(CCACHE_PROGRAM ccache)
IF(CCACHE_PROGRAM)
  MESSAGE(STATUS "CCACHE_PROGRAM FIND")
  SET_PROPERTY(GLOBAL PROPERTY RULE_LAUNCH_COMPLILE ${CCACHE_PROGRAM})
  SET_PROPERTY(GLOBAL PROPERTY RULE_LAUNCH_LINK ${CCACHE_PROGRAM})
ELSE()
  MESSAGE(STATUS "CCACHE_PROGRAM NOT FIND")
ENDIF()

# legacy/CMakeLists.txt 和 mylib/CMakeLists.txt 里还有问题哦！
add_subdirectory(legacy)
add_subdirectory(mylib)

# 这样需要一个个写出所有文件很麻烦，请改成自动批量添加源文件
# set(main_sources "src/main.cpp" "src/other.cpp" "src/dummy.cpp" "src/veryusefulfile.cpp")
AUX_SOURCE_DIRECTORY(src main_sources)

add_executable(main ${main_sources})

# 请改为项目的正确版本（用变量来获取）
target_compile_definitions(main PRIVATE HELLOCMAKE_VERSION=${PROJECT_VERSION})

# （可选）添加 run 作为伪目标方便命令行调用
ADD_CUSTOM_TARGET(RUN COMMAND $<TARGET_FILE:main>)
