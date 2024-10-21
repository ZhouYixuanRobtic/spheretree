set(BOT_LIB_NAME API)
file(GLOB_RECURSE LibSource ./*.cpp)
add_library(${BOT_LIB_NAME} STATIC
        ${LibSource}
)
add_subdirectory(SFWhite)
target_link_libraries(${BOT_LIB_NAME} SFWhite)
