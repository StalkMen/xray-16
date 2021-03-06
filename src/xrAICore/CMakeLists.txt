project(xrAICore)

list(APPEND DIRS
    "."
    "Components"
    "Navigation"
    "Navigation/PatrolPath"
    "Navigation/PathManagers"
    )

add_dir("${DIRS}")

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/..
    ${CMAKE_CURRENT_SOURCE_DIR}/../../Externals/luabind
    ${CMAKE_CURRENT_SOURCE_DIR}/../../sdk/include
    ${SDL_INCLUDE_DIRS}
    )

#foreach(header ${${PROJECT_NAME}__INCLUDES})
#	if (${header} MATCHES _inline.h$)
#		list(REMOVE_ITEM ${PROJECT_NAME}__INCLUDES ${header})
#	endif()
#endforeach()

add_definitions(-DXRAICORE_EXPORTS)
add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME}__SOURCES} ${${PROJECT_NAME}__INCLUDES})

set_target_properties(${PROJECT_NAME} PROPERTIES COTIRE_CXX_PREFIX_HEADER_INIT "pch.hpp")
cotire(${PROJECT_NAME})

set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "")
target_link_libraries(${PROJECT_NAME} xrCore xrEngine xrMiscMath xrAPI xrScriptEngine xrLuabind ${LUA_LIBRARIES})

install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION /usr/lib PERMISSIONS OWNER_READ OWNER_WRITE 
    GROUP_READ 
    WORLD_READ )
