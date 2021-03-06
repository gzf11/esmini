
include_directories (
  ${ROADMANAGER_INCLUDE_DIR}
  ${SCENARIOENGINE_INCLUDE_DIRS}
  ${VIEWER_BASE_INCLUDE_DIR}
  ${OSG_INCLUDE_DIR}
  ${COMMON_MINI_INCLUDE_DIR}
  ${OSI_INCLUDE_DIR}
  ${CONTROLLERS_INCLUDE_DIR}
)

set ( INCLUDES
	Replay.hpp
)

# Replayer target
set (TARGET1 replayer)
add_definitions(-DOSG_LIBRARY_STATIC)
set ( SOURCES
	main.cpp
	Replay.cpp
)
link_directories( ${OSG_LIBRARIES_PATH} )
add_executable ( ${TARGET1} ${SOURCES} ${INCLUDES} )
target_link_libraries ( 
	${TARGET1}
	RoadManager
	ScenarioEngine
	ViewerBase
	CommonMini
	${OSG_LIBRARIES}
	${TIME_LIB}	
)

# dat2csv target
set (TARGET2 dat2csv)
add_executable ( ${TARGET2} dat2csv.cpp Replay.cpp )
target_link_libraries ( ${TARGET2} RoadManager CommonMini ${TIME_LIB} )


# osi_receiver target
set (TARGET3 osireceiver)
add_executable ( ${TARGET3} osi_receiver.cpp )
target_link_libraries ( ${TARGET3} ${OSI_LIBRARIES} ${TIME_LIB} ${SOCK_LIB})

# Install directives

if (UNIX)
  install ( TARGETS ${TARGET1} ${TARGET2} ${TARGET3} DESTINATION "${INSTALL_DIRECTORY}")
else()
  install ( TARGETS ${TARGET1} ${TARGET2} ${TARGET3} CONFIGURATIONS Release DESTINATION "${INSTALL_DIRECTORY}")
  install ( TARGETS ${TARGET1} ${TARGET2} ${TARGET3} CONFIGURATIONS Debug DESTINATION "${INSTALL_DIRECTORY}")
endif (UNIX)
