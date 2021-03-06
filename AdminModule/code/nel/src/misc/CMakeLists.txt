FILE(GLOB SRC *.cpp *.h config_file/*.cpp config_file/*.h)
FILE(GLOB HEADERS ../../include/nel/misc/*.h)

NL_TARGET_LIB(nelmisc ${HEADERS} ${SRC})

IF(WITH_GTK)
  IF(GTK2_FOUND)
    INCLUDE_DIRECTORIES(${GTK2_INCLUDE_DIRS})
    ADD_DEFINITIONS(-DNL_USE_GTK)
    TARGET_LINK_LIBRARIES(nelmisc ${GTK2_LIBRARIES})
  ENDIF()
ENDIF()

IF(UNIX)
  TARGET_LINK_LIBRARIES(nelmisc -lc -ldl)
  IF(APPLE)
    TARGET_LINK_LIBRARIES(nelmisc ${CARBON_FRAMEWORK})
  ELSE()
    TARGET_LINK_LIBRARIES(nelmisc -lrt)
  ENDIF()
ENDIF()

TARGET_LINK_LIBRARIES(nelmisc ${CMAKE_THREAD_LIBS_INIT})
NL_DEFAULT_PROPS(nelmisc "NeL, Library: NeL Misc")

NL_ADD_LIB_SUFFIX(nelmisc)

IF(WITH_PCH)
  ADD_NATIVE_PRECOMPILED_HEADER(nelmisc ${CMAKE_CURRENT_SOURCE_DIR}/stdmisc.h ${CMAKE_CURRENT_SOURCE_DIR}/stdmisc.cpp)
ENDIF()

NL_GEN_PC(nel-misc.pc)

IF((WITH_INSTALL_LIBRARIES AND WITH_STATIC) OR NOT WITH_STATIC)
  INSTALL(TARGETS nelmisc LIBRARY DESTINATION ${NL_LIB_PREFIX} ARCHIVE DESTINATION ${NL_LIB_PREFIX} COMPONENT libraries)
ENDIF()
