# Copyright (C) 2020, Burkhard Stubert (DBA Embedded Use)
cmake_minimum_required(VERSION 3.5)

project(SimpleApp)

option(DEPLOY_QT "Deploy Qt to the target system." ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_VERBOSE_MAKEFILE true)

find_package(Qt5Gui)
find_package(Qt5Qml)
find_package(Qt5Quick)

set(CMAKE_INSTALL_PREFIX ${CMAKE_BINARY_DIR}/final)

set(CMAKE_SKIP_BUILD_RPATH true)
set(CMAKE_BUILD_WITH_INSTALL_RPATH false)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH false)
set(CMAKE_INSTALL_RPATH $ORIGIN $ORIGIN/../qt/lib)

set(CPACK_PACKAGE_NAME "SimpleApp")
set(CPACK_PACKAGE_VENDOR "MyComp")
set(CPACK_VERBATIM_VARIABLES true)
set(CPACK_PACKAGE_VERSION_MAJOR 0)
set(CPACK_PACKAGE_VERSION_MINOR 2)
set(CPACK_PACKAGE_VERSION_PATCH 0)
include(CPack)

add_executable(
    ${PROJECT_NAME}
    main.cpp
    Customer.h
    CustomerManager.h
    qml.qrc
)

target_link_libraries(${PROJECT_NAME}
    Qt5::Gui Qt5::Qml Qt5::Quick
)

macro(add_deployment_file SRC DEST)
    file(RELATIVE_PATH relSrc ${CMAKE_SOURCE_DIR} ${SRC})
    file(APPEND "${CMAKE_BINARY_DIR}/QtCreatorDeployment.txt"
        "${relSrc}:${DEST}\n")
endmacro()

macro(add_deployment_directory SRC DEST)
    file(GLOB_RECURSE files "${SRC}/*")
    foreach(filename ${files})
        get_filename_component(path ${filename} PATH)
        file(RELATIVE_PATH path ${SRC} ${path})
        add_deployment_file("${filename}" "${DEST}/${path}")
    endforeach(filename)
endmacro()

install(TARGETS ${PROJECT_NAME} RUNTIME
    DESTINATION bin)

set(target_install_prefix "/home/benutzer/MyComp")
file(WRITE "${CMAKE_BINARY_DIR}/QtCreatorDeployment.txt"
     "${target_install_prefix}\n")
add_deployment_file(${CMAKE_INSTALL_PREFIX}/bin/${PROJECT_NAME} bin)

if(DEPLOY_QT)
    set(qt_base_dir ${Qt5Core_DIR}/../../..)
    install(DIRECTORY ${qt_base_dir}/lib ${qt_base_dir}/phrasebooks
        ${qt_base_dir}/plugins ${qt_base_dir}/qml ${qt_base_dir}/translations
        DESTINATION qt USE_SOURCE_PERMISSIONS)

    add_deployment_directory("${qt_base_dir}/lib" "qt/lib")
    add_deployment_directory("${qt_base_dir}/phrasebooks" "qt/phrasebooks")
    add_deployment_directory("${qt_base_dir}/plugins" "qt/plugins")
    add_deployment_directory("${qt_base_dir}/qml" "qt/qml")
    add_deployment_directory("${qt_base_dir}/translations" "qt/translations")
endif()
