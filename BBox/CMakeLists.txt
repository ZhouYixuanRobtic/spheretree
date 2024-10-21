set(BOT_LIB_NAME BBox)
file(GLOB_RECURSE LibSource ./*.cpp)
add_library(${BOT_LIB_NAME} STATIC
        ${LibSource}
)

add_subdirectory(MVBB)
add_subdirectory(qHull)

target_link_libraries(${BOT_LIB_NAME} qHull MVBB)

