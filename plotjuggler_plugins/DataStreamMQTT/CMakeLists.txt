
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")
find_package(MOSQUITTO QUIET)

if(MOSQUITTO_FOUND)

  message(STATUS "[MOSQUITTO] found.")
  qt5_wrap_ui(UI_SRC datastream_mqtt.ui)

  set(SRC datastream_mqtt.h datastream_mqtt.cpp mqtt_dialog.h mqtt_dialog.cpp
          mqtt_client.h mqtt_client.cpp)

  add_library(DataStreamMQTT_Mosquitto SHARED ${SRC} ${UI_SRC})

  target_link_libraries(
    DataStreamMQTT_Mosquitto PRIVATE Qt5::Widgets Qt5::Network Qt5::Svg
                                     plotjuggler_base)

  target_compile_definitions(DataStreamMQTT_Mosquitto PRIVATE QT_PLUGIN)

  target_link_libraries(DataStreamMQTT_Mosquitto PRIVATE mosquitto::mosquitto)

  install(TARGETS DataStreamMQTT_Mosquitto
          DESTINATION ${PJ_PLUGIN_INSTALL_DIRECTORY})

else()
  message("[MOSQUITTO] not found. Skipping plugin DataStreamMQTT.")
endif()
