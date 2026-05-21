add_subdirectory(A1)
add_subdirectory(A2)
add_subdirectory(M1)
add_subdirectory(M2)


#[=[
set(Modules_INCLUDE_DIRS ${CMAKE_CURRENT_LIST_DIR}/include CACHE PATH "Modules_INCLUDE_DIRS")
file(GLOB subdirs RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/*)
#自动扫描当前目录下的所有子目录

foreach(m ${subdirs})
    if(IS_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${m})
        if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/${m}/CMakeLists.txt)
            add_subdirectory(${m})
        endif()
    endif()
endforeach()
#]=]
