# Custom AngryGL

OpenGL clone of the [Unity Angry Bots ECS sample project](https://github.com/UnityTechnologies/AngryBots_ECS)

originaly forked from  [AngryGL](url)



# Building

The project is built using qmake or cmake.

**NOTE:** None of the original assets from Unity's project have been included, you'll need to copy them into the appropriate directories if you want to run the project yourself. The .gitignore file can be a good guide for this.

## Build from Source

### Dependencies

* [irrKlang](https://www.ambiera.com/irrklang/index.html) 
* [stb image](https://github.com/nothings/stb/blob/master/stb_image.h)
* [assimp](https://github.com/assimp/assimp)
* [glad](https://github.com/Dav1dde/glad)
* [ThreadPool](https://github.com/progschj/ThreadPool)
* [glm](https://github.com/g-truc/glm)
* [glfw](https://github.com/glfw/glfw)

### Build Dependencies

- Qmake
- CMAKE

### Build Steps camke
```
$ mkdir build
$ cd build
$ cmake --build . -- -j8

```
### Build Steps qmake
```
$ qmake
$ make
```

