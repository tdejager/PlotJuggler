add_definitions(${QT_DEFINITIONS})
add_definitions(-DQT_PLUGIN)

QT5_WRAP_UI ( UI_SRC  dialog_mcap.ui  )


if(NOT TARGET mcap)
    ### mcap ###
    CPMAddPackage(
        NAME mcap
        GITHUB_REPOSITORY foxglove/mcap
        GIT_TAG releases/cpp/v2.0.0
        DOWNLOAD_ONLY YES
    )
    # mcap has no CMake support, so we create our own target
    add_library(mcap INTERFACE)
    target_include_directories(mcap INTERFACE "${mcap_SOURCE_DIR}/cpp/mcap/include")
    target_link_libraries(mcap INTERFACE
        ${LZ4_LIBRARIES}
        ${ZSTD_LIBRARIES}
    )
 endif()
########################################


add_library(DataLoadMCAP SHARED
    mcap.cpp
    dataload_mcap.cpp
    dialog_mcap.cpp
    ${UI_SRC}  )

target_include_directories(DataLoadMCAP PRIVATE
    $<BUILD_INTERFACE:${zstd_SOURCE_DIR}/lib>
    $<BUILD_INTERFACE:${lz4_SOURCE_DIR}/lib>)

target_link_libraries(DataLoadMCAP PRIVATE
    ${Qt5Widgets_LIBRARIES}
    ${Qt5Xml_LIBRARIES}
    ${Qt5Concurrent_LIBRARIES}
    mcap
    plotjuggler_base )

install(TARGETS DataLoadMCAP DESTINATION ${PJ_PLUGIN_INSTALL_DIRECTORY}  )
