michel@MichelUbuntu:~$ export GITHUB_USERNAME=MichelsonIU815
michel@MichelUbuntu:~$ alias gsed=sed
michel@MichelUbuntu:~$ cd ${GITHUB_USERNAME}/workspace
michel@MichelUbuntu:~/MichelsonIU815/workspace$ pushd .
~/MichelsonIU815/workspace ~/MichelsonIU815/workspace
michel@MichelUbuntu:~/MichelsonIU815/workspace$ source scripts/activate
michel@MichelUbuntu:~/MichelsonIU815/workspace$ 
git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07
Клонирование в «projects/lab07»...
remote: Enumerating objects: 61, done.
remote: Counting objects: 100% (61/61), done.
remote: Compressing objects: 100% (35/35), done.
remote: Total 61 (delta 20), reused 57 (delta 18), pack-reused 0 (from 0)
Получение объектов: 100% (61/61), 21.53 КиБ | 489.00 КиБ/с, готово.
Определение изменений: 100% (20/20), готово.
michel@MichelUbuntu:~/MichelsonIU815/workspace$ cd projects/lab07
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ git remote remove origin
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cat > CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.14)
> project(print)
> 
> 
include("cmake/HunterGate.cmake")
> HunterGate(
>     URL "https://github.com/cpp-pm/hunter/archive/v0.23.314.tar.gz"
>     SHA1 "572541b3a9e2c1ccb0c0e552f6dc12219c0d6a0b"
>     LOCAL
> )
> 
> option(BUILD_TESTS "Build tests" OFF)
> 
> 
hunter_add_package(GTest)
> find_package(GTest REQUIRED)

add_library(print STATIC src/print.cpp)
target_include_directories(print PUBLIC include)

if(BUILD_TESTS)
    enable_testing()
    add_executable(check tests/test.cpp)
    target_link_libraries(check print GTest::GTest GTest::Main)
    add_test(NAME check COMMAND check)
endif()

add_executable(demo demo/main.cpp)
target_link_libraries(demo print)
install(TARGETS demo RUNTIME DESTINATION bin)
EOF
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ mkdir -p cmake
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ wget https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake -O cmake/HunterGate.cmake
--2025-04-21 13:14:46--  https://raw.githubusercontent.com/cpp-pm/gate/master/cmake/HunterGate.cmake
Распознаётся raw.githubusercontent.com (raw.githubusercontent.com)… 185.199.109.133, 185.199.110.133, 185.199.111.133, ...
Подключение к raw.githubusercontent.com (raw.githubusercontent.com)|185.199.109.133|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 200 OK
Длина: 17231 (17K) [text/plain]
Сохранение в: ‘cmake/HunterGate.cmake’

cmake/HunterGate.cm 100%[===================>]  16,83K  --.-KB/s    за 0s      

2025-04-21 13:14:46 (96,2 MB/s) - ‘cmake/HunterGate.cmake’ сохранён [17231/17231]

michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ [ -d "third-party/gtest" ] && git rm -rf third-party/gtest
rm 'third-party/gtest'
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ mkdir -p include src tests demo
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ touch include/print.hpp src/print.cpp tests/test.cpp
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cat > demo/main.cpp <<'EOF'
> #include <print.hpp>
#include <iostream>
#include <fstream>
#include <cstdlib>

int main(int argc, char* argv[]) {
    const char* log_path = std::getenv("LOG_PATH");
    if (log_path == nullptr) {
        std::cerr << "undefined environment variable: LOG_PATH" << std::endl;
        return 1;
    }
    std::string text;
    while (std::cin >> text) {
        std::ofstream out{log_path, std::ios_base::app};
        print(text, out);
        out << std::endl;
    }
}
EOF
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ mkdir -p cmake/Hunter
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cat > cmake/Hunter/config.cmake <<'EOF'
> hunter_config(GTest VERSION 1.11.0)
> EOF
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ rm -rf _builds
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cmake -H. -B_builds -DBUILD_TESTS=ON
-- The C compiler identification is GNU 9.5.0
-- The CXX compiler identification is GNU 9.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done

[hunter ** FATAL ERROR **] Please set HunterGate *before* 'project' command. 
[hunter ** FATAL ERROR **] Detected project: print
[hunter ** FATAL ERROR **] [Directory:/home/michel/MichelsonIU815/workspace/projects/lab07]

------------------------------ ERROR ------------------------------
    https://hunter.readthedocs.io/en/latest/reference/errors/error.huntergate.before.project.html
-------------------------------------------------------------------

CMake Error at cmake/HunterGate.cmake:88 (message):
Call Stack (most recent call first):
  cmake/HunterGate.cmake:112 (hunter_gate_error_page)
  cmake/HunterGate.cmake:401 (hunter_gate_fatal_error)
  CMakeLists.txt:6 (HunterGate)


-- Configuring incomplete, errors occurred!
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cat > CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.14)
  EOF
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cat > CMakeLists.txt <<'EOF'
> cmake_minimum_required(VERSION 3.14)
> 
> include("cmake/HunterGate.cmake")
> HunterGate(
>     URL "https://github.com/cpp-pm/hunter/archive/v0.23.314.tar.gz"
>     SHA1 "572541b3a9e2c1ccb0c0e552f6dc12219c0d6a0b"
>     LOCAL
> )
> 
> project(print)
> 
> option(BUILD_TESTS "Build tests" OFF)
> 
> hunter_add_package(GTest)
> find_package(GTest REQUIRED)
> 
> add_library(print STATIC src/print.cpp)
> target_include_directories(print PUBLIC include)
> 
> if(BUILD_TESTS)
>     enable_testing()
>     add_executable(check tests/test.cpp)
>     target_link_libraries(check print GTest::GTest GTest::Main)
>     add_test(NAME check COMMAND check)
> endif()
> 
> add_executable(demo demo/main.cpp)
> target_link_libraries(demo print)
> install(TARGETS demo RUNTIME DESTINATION bin)
> EOF
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ rm -rf _builds ~/.hunter
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cmake -H. -B_builds -DBUILD_TESTS=ON
-- [hunter] Initializing Hunter workspace (572541b3a9e2c1ccb0c0e552f6dc12219c0d6a0b)
-- [hunter]   https://github.com/cpp-pm/hunter/archive/v0.23.314.tar.gz
-- [hunter]   -> /home/michel/.hunter/_Base/Download/Hunter/0.23.314/572541b
CMake Error at Build/Hunter-prefix/src/Hunter-stamp/download-Hunter.cmake:170 (message):
  Each download failed!

    
    


gmake[2]: *** [CMakeFiles/Hunter.dir/build.make:99: Hunter-prefix/src/Hunter-stamp/Hunter-download] Ошибка 1
gmake[1]: *** [CMakeFiles/Makefile2:83: CMakeFiles/Hunter.dir/all] Ошибка 2
gmake: *** [Makefile:91: all] Ошибка 2

[hunter ** INTERNAL **] Build project failed
[hunter ** INTERNAL **] [Directory:/home/michel/MichelsonIU815/workspace/projects/lab07]

------------------------------ ERROR ------------------------------
    https://hunter.readthedocs.io/en/latest/reference/errors/error.internal.html
-------------------------------------------------------------------

CMake Error at cmake/HunterGate.cmake:88 (message):
Call Stack (most recent call first):
  cmake/HunterGate.cmake:98 (hunter_gate_error_page)
  cmake/HunterGate.cmake:347 (hunter_gate_internal_error)
  cmake/HunterGate.cmake:511 (hunter_gate_download)
  CMakeLists.txt:4 (HunterGate)


-- Configuring incomplete, errors occurred!
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ wget https://github.com/cpp-pm/hunter/archive/v0.23.314.tar.gz -O /tmp/hunter.tar.gz
--2025-04-21 13:23:31--  https://github.com/cpp-pm/hunter/archive/v0.23.314.tar.gz
Распознаётся github.com (github.com)… 140.82.121.3
Подключение к github.com (github.com)|140.82.121.3|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 302 Found
Адрес: https://codeload.github.com/cpp-pm/hunter/tar.gz/refs/tags/v0.23.314 [переход]
--2025-04-21 13:23:31--  https://codeload.github.com/cpp-pm/hunter/tar.gz/refs/tags/v0.23.314
Распознаётся codeload.github.com (codeload.github.com)… 140.82.121.10
Подключение к codeload.github.com (codeload.github.com)|140.82.121.10|:443... соединение установлено.
HTTP-запрос отправлен. Ожидание ответа… 200 OK
Длина: нет данных [application/x-gzip]
Сохранение в: ‘/tmp/hunter.tar.gz’

/tmp/hunter.tar.gz      [    <=>             ]   2,60M  3,81MB/s    за 0,7s    

2025-04-21 13:23:33 (3,81 MB/s) - ‘/tmp/hunter.tar.gz’ сохранён [2725603]

michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ rm -rf ~/.hunter _builds
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cmake -H. -B_builds -DBUILD_TESTS=ON
-- [hunter] Initializing Hunter workspace (572541b3a9e2c1ccb0c0e552f6dc12219c0d6a0b)
-- [hunter]   https://github.com/cpp-pm/hunter/archive/v0.23.314.tar.gz
-- [hunter]   -> /home/michel/.hunter/_Base/Download/Hunter/0.23.314/572541b
CMake Error at Build/Hunter-prefix/src/Hunter-stamp/download-Hunter.cmake:170 (message):
  Each download failed!

    
    


gmake[2]: *** [CMakeFiles/Hunter.dir/build.make:99: Hunter-prefix/src/Hunter-stamp/Hunter-download] Ошибка 1
gmake[1]: *** [CMakeFiles/Makefile2:83: CMakeFiles/Hunter.dir/all] Ошибка 2
gmake: *** [Makefile:91: all] Ошибка 2

[hunter ** INTERNAL **] Build project failed
[hunter ** INTERNAL **] [Directory:/home/michel/MichelsonIU815/workspace/projects/lab07]

------------------------------ ERROR ------------------------------
    https://hunter.readthedocs.io/en/latest/reference/errors/error.internal.html
-------------------------------------------------------------------

CMake Error at cmake/HunterGate.cmake:88 (message):
Call Stack (most recent call first):
  cmake/HunterGate.cmake:98 (hunter_gate_error_page)
  cmake/HunterGate.cmake:347 (hunter_gate_internal_error)
  cmake/HunterGate.cmake:511 (hunter_gate_download)
  CMakeLists.txt:4 (HunterGate)


-- Configuring incomplete, errors occurred!
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ git clone https://github.com/cpp-pm/hunter ~/hunter
Клонирование в «/home/michel/hunter»...
remote: Enumerating objects: 53004, done.
remote: Counting objects: 100% (861/861), done.
remote: Compressing objects: 100% (231/231), done.
remote: Total 53004 (delta 683), reused 630 (delta 630), pack-reused 52143 (from 3)
Получение объектов: 100% (53004/53004), 13.81 МиБ | 5.20 МиБ/с, готово.
Определение изменений: 100% (33126/33126), готово.
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ export HUNTER_ROOT=~/hunter
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ rm -rf _builds
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cmake -H. -B_builds -DBUILD_TESTS=ON
-- The C compiler identification is GNU 9.5.0
-- The CXX compiler identification is GNU 9.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /home/michel/hunter
-- [hunter] [ Hunter-ID: xxxxxxx | Toolchain-ID: d43c700 | Config-ID: 37d6cfd ]
-- [hunter] GTEST_ROOT: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Install (ver.: 1.11.0)
-- [hunter] Building GTest
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/cache.cmake
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/args.cmake
-- The C compiler identification is GNU 9.5.0
-- The CXX compiler identification is GNU 9.5.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (0.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Build
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/home/michel/hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/google/googletest/archive/release-1.11.0.tar.gz'
-- [download 1% complete]
-- [download 2% complete]
-- [download 3% complete]
-- [download 4% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 10% complete]
-- [download 11% complete]
-- [download 14% complete]
-- [download 15% complete]
-- [download 16% complete]
-- [download 17% complete]
-- [download 18% complete]
-- [download 19% complete]
-- [download 20% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 23% complete]
-- [download 24% complete]
-- [download 26% complete]
-- [download 29% complete]
-- [download 33% complete]
-- [download 34% complete]
-- [download 38% complete]
-- [download 41% complete]
-- [download 44% complete]
-- [download 47% complete]
-- [download 48% complete]
-- [download 52% complete]
-- [download 54% complete]
-- [download 56% complete]
-- [download 57% complete]
-- [download 58% complete]
-- [download 60% complete]
-- [download 61% complete]
-- [download 62% complete]
-- [download 63% complete]
-- [download 64% complete]
-- [download 65% complete]
-- [download 66% complete]
-- [download 69% complete]
-- [download 72% complete]
-- [download 75% complete]
-- [download 78% complete]
-- [download 81% complete]
-- [download 84% complete]
-- [download 85% complete]
-- [download 86% complete]
-- [download 88% complete]
-- [download 89% complete]
-- [download 100% complete]
-- verifying file...
       file='/home/michel/hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
-- Downloading... done
-- extracting...
     src='/home/michel/hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
     dst='/home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No update step for 'GTest-Release'
[ 25%] No patch step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/cache.cmake
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/args.cmake
CMake Deprecation Warning at CMakeLists.txt:4 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- The C compiler identification is GNU 9.5.0
-- The CXX compiler identification is GNU 9.5.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at googlemock/CMakeLists.txt:45 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at googletest/CMakeLists.txt:56 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Found Python: /usr/bin/python3 (found version "3.12.3") found components: Interpreter 
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Configuring done (0.3s)
-- Generating done (0.0s)
-- Build files have been written to: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtest.a
[ 25%] Built target gtest
[ 50%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_main.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmock.a
[ 75%] Built target gmock
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_main.a
[100%] Built target gmock_main
[ 43%] Performing install step for 'GTest-Release'
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgmock.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgtest.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/home/michel/hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
-- File already exists and hash match (skip download):
  file='/home/michel/hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
  SHA1='7b100bb68db8df1060e178c495f3cbe941c9b058'
-- extracting...
     src='/home/michel/hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
     dst='/home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No update step for 'GTest-Debug'
[ 75%] No patch step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/cache.cmake
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/args.cmake
CMake Deprecation Warning at CMakeLists.txt:4 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- The C compiler identification is GNU 9.5.0
-- The CXX compiler identification is GNU 9.5.0
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Deprecation Warning at googlemock/CMakeLists.txt:45 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


CMake Deprecation Warning at googletest/CMakeLists.txt:56 (cmake_minimum_required):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Found Python: /usr/bin/python3 (found version "3.12.3") found components: Interpreter 
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE  
-- Configuring done (0.3s)
-- Generating done (0.0s)
-- Build files have been written to: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtestd.a
[ 25%] Built target gtest
[ 37%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 50%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_maind.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmockd.a
[ 75%] Built target gmock
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgmockd.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgmock_maind.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgtestd.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/libgtest_maind.a
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Build/GTest)
-- [hunter] Cache saved: /home/michel/hunter/_Base/Cache/raw/86e534383bf3277035d001fc476b2a6d1f77b9b0.tar.bz2
CMake Warning (dev) at CMakeLists.txt:15 (find_package):
  Policy CMP0144 is not set: find_package uses upper-case <PACKAGENAME>_ROOT
  variables.  Run "cmake --help-policy CMP0144" for policy details.  Use the
  cmake_policy command to set the policy and suppress this warning.

  CMake variable GTEST_ROOT is set to:

    /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Install

  Environment variable GTEST_ROOT is set to:

    /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Install

  For compatibility, find_package is ignoring the variable, but code in a
  .cmake module might still use it.
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found GTest: /home/michel/hunter/_Base/xxxxxxx/d43c700/37d6cfd/Install/lib/cmake/GTest/GTestConfig.cmake (found version "1.11.0")  
-- Configuring done (14.3s)
-- Generating done (0.0s)
-- Build files have been written to: /home/michel/MichelsonIU815/workspace/projects/lab07/_builds
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cmake --build _builds
[ 16%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[ 33%] Linking CXX static library libprint.a
[ 33%] Built target print
[ 50%] Building CXX object CMakeFiles/check.dir/tests/test.cpp.o
[ 66%] Linking CXX executable check
[ 66%] Built target check
[ 83%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
/usr/bin/ld: CMakeFiles/demo.dir/demo/main.cpp.o: в функции «main»:
main.cpp:(.text+0xec): undefined reference to `print(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::basic_ofstream<char, std::char_traits<char> >&)'
collect2: error: ld returned 1 exit status
gmake[2]: *** [CMakeFiles/demo.dir/build.make:98: demo] Ошибка 1
gmake[1]: *** [CMakeFiles/Makefile2:139: CMakeFiles/demo.dir/all] Ошибка 2
gmake: *** [Makefile:146: all] Ошибка 2
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ cmake --build _builds
[ 16%] Building CXX object CMakeFiles/print.dir/src/print.cpp.o
[ 33%] Linking CXX static library libprint.a
[ 33%] Built target print
[ 50%] Linking CXX executable check
[ 66%] Built target check
[ 83%] Linking CXX executable demo
[100%] Built target demo
michel@MichelUbuntu:~/MichelsonIU815/workspace/projects/lab07$ if [ -f "_builds/check" ]; then
>     cmake --build _builds --target test
> fi
Running tests...
Test project /home/michel/MichelsonIU815/workspace/projects/lab07/_builds
    Start 1: check
1/1 Test #1: check ............................   Passed    0.00 sec

100% tests passed, 0 tests failed out of 1

Total Test time (real) =   0.00 sec
