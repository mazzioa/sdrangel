project(testmi)

set(testmi_SOURCES
	testmi.cpp
	testmiplugin.cpp
	testmithread.cpp
    testmisettings.cpp
    testmiwebapiadapter.cpp
)

set(testmi_HEADERS
	testmi.h
	testmiplugin.h
	testmithread.h
    testmisettings.h
    testmiwebapiadapter.h
)

include_directories(
	${CMAKE_SOURCE_DIR}/swagger/sdrangel/code/qt5/client
)

if (NOT SERVER_MODE)
    set (testmi_SOURCES
        ${testmi_SOURCES}
        testmigui.cpp
        testmigui.ui
    )
    set(testmi_HEADERS
        ${testmi_HEADERS}
        testsourcegui.h
    )
    set(TARGET_NAME mimotestmi)
    set(TARGET_LIB "Qt5::Widgets")
    set(TARGET_LIB_GUI "sdrgui")
    set(INSTALL_FOLDER ${INSTALL_PLUGINS_DIR})
else()
    set(TARGET_NAME mimotestmisrv)
    set(TARGET_LIB "")
    set(TARGET_LIB_GUI "")
    set(INSTALL_FOLDER ${INSTALL_PLUGINSSRV_DIR})
endif()

add_library(${TARGET_NAME} SHARED
	${testmi_SOURCES}
)

target_link_libraries(${TARGET_NAME}
    Qt5::Core
    ${TARGET_LIB}
	sdrbase
	${TARGET_LIB_GUI}
    swagger
)

install(TARGETS ${TARGET_NAME} DESTINATION ${INSTALL_FOLDER})
