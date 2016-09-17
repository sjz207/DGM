file(GLOB VIS_INCLUDE	${PROJECT_SOURCE_DIR}/include/VIS.h)
file(GLOB VIS_SOURCES	"*.cpp")
file(GLOB VIS_HEADERS	"*.h")

# Create named folders for the sources within the .vcproj
# Empty name lists them directly under the .vcproj
source_group("Include" FILES ${VIS_INCLUDE})
source_group("Source Files" FILES ${VIS_SOURCES} ${VIS_HEADERS}) 

# Properties -> C/C++ -> General -> Additional Include Directories
include_directories(${PROJECT_SOURCE_DIR}/include
					${PROJECT_SOURCE_DIR}/modules
					${OpenCV_INCLUDE_DIRS} 
					${OpenCV_INCLUDE_DIRS}2
				)
  
# Properties -> Linker -> General -> Additional Library Directories
link_directories(${CMAKE_LIBRARY_OUTPUT_DIRECTORY})  
  
# Set Properties -> General -> Configuration Type to Dynamic Library(.dll)
add_library(VIS SHARED ${VIS_INCLUDE} ${VIS_SOURCES} ${VIS_HEADERS})
add_dependencies(VIS DGM)
if(USE_OPENGL)
	add_dependencies(VIS glfw)
endif()
 
# Properties -> Linker -> Input -> Additional Dependencies
target_link_libraries(VIS ${OpenCV_LIBS} ${DGM_LIB})
if(USE_OPENGL)
	target_link_libraries(VIS glfw ${OPENGL_gl_LIBRARY})
endif()

 
set_target_properties(VIS PROPERTIES OUTPUT_NAME_RELEASE vis${DGM_VERSION_MAJOR}${DGM_VERSION_MINOR}${DGM_VERSION_PATCH})
set_target_properties(VIS PROPERTIES OUTPUT_NAME_DEBUG vis${DGM_VERSION_MAJOR}${DGM_VERSION_MINOR}${DGM_VERSION_PATCH}d)

#install
install(TARGETS VIS 
	RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin 
	LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
	ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
install(FILES ${VIS_INCLUDE} DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(FILES ${VIS_HEADERS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include/VIS)

# Creates folder "Modules" and adds target project 
set_target_properties(VIS PROPERTIES FOLDER "Modules")
 