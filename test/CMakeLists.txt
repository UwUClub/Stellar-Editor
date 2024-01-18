cmake_minimum_required(VERSION 3.15...3.23)

project(CmakeConfigPackageTests LANGUAGES CXX)

find_package(Qt6 COMPONENTS Core Gui Widgets Test REQUIRED)

set(CMAKE_AUTOMOC ON)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ---- Test as standalone project the exported config package ----

if (PROJECT_IS_TOP_LEVEL OR TEST_INSTALLED_VERSION)
    enable_testing()

    find_package(myproject CONFIG REQUIRED) # for intro, project_options, ...

    if (NOT TARGET myproject_options)
        message(FATAL_ERROR "Requiered config package not found!")
        return() # be strictly paranoid for Template Janitor github action! CK
    endif ()
endif ()

# ---- Dependencies ----

qt_standard_project_setup()

qt_add_executable(tests
        TestQString.cpp
)

target_link_libraries(
        tests
        PRIVATE
        myproject::myproject_warnings
        myproject::myproject_options
        Qt6::Core
        Qt6::Gui
        Qt6::Widgets
        Qt6::Test
)

add_test(tests tests)

if (WIN32 AND BUILD_SHARED_LIBS)
    add_custom_command(
            TARGET tests
            PRE_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_RUNTIME_DLLS:tests> $<TARGET_FILE_DIR:tests>
            COMMAND_EXPAND_LISTS)
endif ()
