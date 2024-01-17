qt_standard_project_setup()

qt_add_executable(SollarEngine
        main.cpp
)

target_link_libraries(SollarEngine PRIVATE Qt6::Core Qt6::Gui Qt6::Widgets)
