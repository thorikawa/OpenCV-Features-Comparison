cmake_minimum_required(VERSION 2.8)
project(OPENCV_FEATURES_COMPARISON)

find_package(OpenCV REQUIRED)
find_package(OpenMP)

if(OPENMP_FOUND)
  MESSAGE("OpenMP found")
  if(UNIX)
    SET(CMAKE_C_FLAGS "-O3 -Wall -Wextra -Wunused-variable -msse2 -msse3 -msse4 -ffast-math")
    SET(CMAKE_CXX_FLAGS "-O3 -Wall -Wextra -Wunused-variable -msse2 -msse3 -msse4 -ffast-math")
  endif(UNIX)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
else(OPENMP_FOUND)
  MESSAGE("OpenMP not found")
  if(UNIX)
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -Wno-unknown-pragmas -Wunused-variable -msse2 -msse3 -msse4 -ffast-math")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -Wno-unknown-pragmas -Wunused-variable -msse2 -msse3 -msse4 -ffast-math")
  endif(UNIX)
endif(OPENMP_FOUND)

include_directories(${OpenCV_INCLUDE_DIR})

link_directories(${OpenCV_LIBRARY_DIR})

add_executable(test_opencv_features2d main.cpp ImageTransformation.hpp ImageTransformation.cpp FeatureAlgorithm.hpp FeatureAlgorithm.cpp AlgorithmEstimation.hpp AlgorithmEstimation.cpp CollectedStatistics.hpp
CollectedStatistics.cpp akaze_features.cpp akaze_fed.cpp akaze_nldiffusion_functions.cpp akaze_utils.cpp AKAZE.cpp 
kaze.cpp kaze_nldiffusion_functions.cpp kaze_config.cpp kaze_features.cpp kaze_ipoint.cpp kaze_utils.cpp 
)

target_link_libraries( test_opencv_features2d ${OpenCV_LIBRARIES} )
      
install (TARGETS test_opencv_features2d DESTINATION bin)
