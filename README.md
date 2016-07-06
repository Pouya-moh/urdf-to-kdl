# urdf-to-kdl
This is a URDF parser with KDL output which is based on [ROS stack](http://wiki.ros.org/kdl_parser) and code by *Wim Meeussen*. All I did, was to remove stuff related to ROS.

### How to use
```cmake
add_subdirectory(urdf-to-kdl) # or however you call it
include_directories(${KDL_Parser_INCLUDE_DIRS})
target_link_libraries(KDL_Parser)
```

### To test
Run cmake by passing `-DBUILD_TEST=ON`. It will create an executable which accepts a urdf as command line argument.

### Know issues
1. This accepts only a path to a urdf file. Loading from server is not supported
2. cmake lacks install
3. more?

### TODO
1. Remove unused methods and class members
2. replace `std::cout` with `std::cerr` when needed.
3. add install to cmake
