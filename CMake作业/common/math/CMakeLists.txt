add_library(math STATIC src/Math.cpp)

target_include_directories(math 
PUBLIC
    include
    ${OpenCV_INCLUDE_DIRS}
)

target_link_libraries(math
    PUBLIC ${OpenCV_LIBS}
)
