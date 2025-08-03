cmake_minimum_required(VERSION 3.16)
project(angrygl)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Sources
set(SOURCES
    main.cpp
    bullet_store.cpp
    enemy_spawner.cpp
    lib/glad/src/glad.cpp
    lib/stb/image.cpp
    model.cpp
    player_mesh.cpp
    player_model.cpp
    shader.cpp
)

# Headers
set(HEADERS
    aabb.h
    bullet_store.h
    capsule.h
    enemy.h
    enemy_spawner.h
    geom.h
    lib/ThreadPool.h
    lib/glad/include/KHR/khrplatform.h
    lib/glad/include/glad/glad.h
    lib/stb/image.h
    lib/stb/stb_image.h
    model.h
    player_mesh.h
    player_model.h
    shader.h
    spritesheet.h
    texture.h
    vertex.h
)

# Shader and resource files
set(DISTFILES
    ../.gitignore
    ../LICENSE
    ../README.md
    shaders/basic_texture_shader.frag
    shaders/basic_texture_shader.vert
    shaders/basicer_shader.frag
    shaders/basicer_shader.vert
    shaders/blur_shader.frag
    shaders/depth_shader.frag
    shaders/depth_shader.vert
    shaders/floor_shader.frag
    shaders/geom_shader.vert
    shaders/geom_shader2.vert
    shaders/instanced_texture_shader.vert
    shaders/player_shader.frag
    shaders/player_shader.vert
    shaders/redshader.frag
    shaders/redshader.vert
    shaders/sprite_shader.frag
    shaders/texture_merge_shader.frag
    shaders/texture_shader.frag
    shaders/wiggly_shader.vert
)

# Include directories
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/../include
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/glad/include
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/stb
)

# Add executable
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# Platform-specific settings
if(WIN32)
    foreach(dll ${DLL_FILES})
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different ${dll} $<TARGET_FILE_DIR:${PROJECT_NAME}>

    )
    endforeach()
    # Windows-specific linking
    target_link_libraries(${PROJECT_NAME}
        ${CMAKE_CURRENT_BINARY_DIR}/../../../libs/glfw/glfw3dll.lib
        ${CMAKE_CURRENT_BINARY_DIR}/../../../libs/assimp/lib/assimp-vc140-mt.lib
        ${CMAKE_CURRENT_BINARY_DIR}/../../../libs/irrklang/irrKlang.lib
        opengl32
        glu32
    )

    # DLLs to copy
    set(DLL_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/../libs/glfw/glfw3.dll
        ${CMAKE_CURRENT_SOURCE_DIR}/../libs/irrklang/ikpMP3.dll
        ${CMAKE_CURRENT_SOURCE_DIR}/../libs/irrklang/irrKlang.dll
    )



else() # Unix/Linux/Mac
    find_package(OpenGL REQUIRED)
    find_package(glfw3 REQUIRED)
    find_package(assimp REQUIRED)

    target_link_libraries(${PROJECT_NAME}
        glfw
        ${OPENGL_LIBRARIES}
        ${ASSIMP_LIBRARIES}
        # IrrKlang is Windows only; remove or handle separately for Unix
    )
endif()

# Copy shader files to output
foreach(file IN LISTS DISTFILES)
    configure_file(${file} ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${file} COPYONLY)
endforeach()
