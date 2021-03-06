## ======================================================================== ##
## Copyright 2009-2016 Intel Corporation                                    ##
##                                                                          ##
## Licensed under the Apache License, Version 2.0 (the "License");          ##
## you may not use this file except in compliance with the License.         ##
## You may obtain a copy of the License at                                  ##
##                                                                          ##
##     http://www.apache.org/licenses/LICENSE-2.0                           ##
##                                                                          ##
## Unless required by applicable law or agreed to in writing, software      ##
## distributed under the License is distributed on an "AS IS" BASIS,        ##
## WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. ##
## See the License for the specific language governing permissions and      ##
## limitations under the License.                                           ##
## ======================================================================== ##

SOURCE_GROUP("Source Files\\bvh4" "/bvh4/" )
SOURCE_GROUP("Source Files\\bvh8" "/bvh8/" )
SOURCE_GROUP("Source Files\\geometry" "/geometry/" )
SOURCE_GROUP("Source Files\\builders" "/builders.*/" )
SOURCE_GROUP("Source Files\\common" "/kernels/common/" )
SOURCE_GROUP("Source Files\\tasking" "/tasking/" )
SOURCE_GROUP("Source Files\\algorithms" "/algorithms/")
SOURCE_GROUP("" FILES embree.rc)

IF (TASKING_INTERNAL)
  ADD_DEFINITIONS(-DTASKING_TBB_INTERNAL)
ENDIF()

IF (TASKING_TBB)
  ADD_DEFINITIONS(-DTASKING_TBB)
  SET(EMBREE_LIBRARIES ${EMBREE_LIBRARIES} ${TBB_LIBRARIES})
ENDIF()

SET(EMBREE_LIBRARY_FILES
 
   embree.rc

  ../../common/tasking/taskscheduler_tbb.cpp
  ../../common/tasking/tasksys.cpp

  ../algorithms/parallel_for.cpp
  ../algorithms/parallel_reduce.cpp
  ../algorithms/parallel_prefix_sum.cpp
  ../algorithms/parallel_for_for.cpp
  ../algorithms/parallel_for_for_prefix_sum.cpp
  ../algorithms/sort.cpp
  ../algorithms/pset.cpp
  ../algorithms/pmap.cpp
  ../algorithms/prefix.cpp

  ../common/device.cpp
  ../common/stat.cpp
  ../common/globals.cpp
  ../common/acceln.cpp
  ../common/accelset.cpp
  ../common/state.cpp
  ../common/rtcore.cpp
  ../common/buffer.cpp
  ../common/scene.cpp
  ../common/geometry.cpp
  ../common/scene_user_geometry.cpp
  ../common/scene_instance.cpp
  ../common/scene_geometry_instance.cpp
  ../common/scene_triangle_mesh.cpp
  ../common/scene_bezier_curves.cpp
  ../common/scene_subdiv_mesh.cpp
  ../common/raystream_log.cpp
  ../common/subdiv/tessellation_cache.cpp
  ../common/subdiv/subdivpatch1base.cpp
  ../common/subdiv/catmullclark_coefficients.cpp

  geometry/primitive.cpp
  geometry/instance_intersector1.cpp
  geometry/instance_intersector.cpp
  geometry/grid_soa.cpp	
  ../common/subdiv/subdivpatch1base_eval.cpp	
  builders/primrefgen.cpp

  bvh/bvh.cpp
  bvh/bvh_statistics.cpp
  bvh/bvh4_factory.cpp
  bvh/bvh8_factory.cpp

  bvh4/bvh4_rotate.cpp
  bvh4/bvh4_refit.cpp
  bvh/bvh_builder.cpp
  bvh4/bvh4_builder_hair.cpp
  bvh4/bvh4_builder_morton.cpp
  bvh/bvh_builder_sah.cpp
  bvh4/bvh4_builder_twolevel.cpp
  bvh4/bvh4_builder_instancing.cpp
  bvh4/bvh4_builder_subdiv.cpp

  bvh/bvh_intersector1.cpp)

IF (RTCORE_RAY_PACKETS)
  SET(EMBREE_LIBRARY_FILES ${EMBREE_LIBRARY_FILES}

  ../common/rtcore_ispc.cpp
  ../common/rtcore_ispc.ispc

  bvh/bvh_intersector_single.cpp
  bvh/bvh_intersector_hybrid.cpp)
ENDIF()

SET(EMBREE_LIBRARY_FILES_SSE42
    geometry/grid_soa.cpp
    ../common/subdiv/subdivpatch1base_eval.cpp
    bvh/bvh_intersector1.cpp)

IF (RTCORE_RAY_PACKETS)
    SET(EMBREE_LIBRARY_FILES_SSE42 ${EMBREE_LIBRARY_FILES_SSE42}
    bvh/bvh_intersector_single.cpp
    bvh/bvh_intersector_hybrid.cpp)
ENDIF()

IF (TARGET_SSE42 AND EMBREE_LIBRARY_FILES_SSE42)
 ADD_DEFINITIONS(-D__TARGET_SSE42__)
ENDIF()

SET(EMBREE_LIBRARY_FILES_AVX
    
    ../common/scene_subdiv_mesh_avx.cpp

    geometry/primitive.cpp
    geometry/instance_intersector1.cpp
    
    geometry/grid_soa.cpp
    ../common/subdiv/subdivpatch1base_eval.cpp
    builders/primrefgen.avx.cpp

    bvh4/bvh4_rotate.cpp
    bvh4/bvh4_refit.avx.cpp
    bvh/bvh_builder.avx.cpp
    bvh4/bvh4_builder_hair.avx.cpp
    bvh4/bvh4_builder_morton.avx.cpp
    bvh/bvh_builder_sah.avx.cpp
    bvh4/bvh4_builder_twolevel.avx.cpp
    bvh4/bvh4_builder_instancing.avx.cpp
    bvh4/bvh4_builder_subdiv.avx.cpp
    bvh/bvh_intersector1.cpp
    
    bvh/bvh.cpp
    bvh/bvh_statistics.cpp
    )

IF (RTCORE_RAY_PACKETS)
  SET(EMBREE_LIBRARY_FILES_AVX ${EMBREE_LIBRARY_FILES_AVX}

    geometry/instance_intersector.cpp

    bvh/bvh_intersector_single.cpp
    bvh/bvh_intersector_hybrid.cpp)
ENDIF()

IF (TARGET_AVX AND EMBREE_LIBRARY_FILES_AVX)
 ADD_DEFINITIONS(-D__TARGET_AVX__)
ENDIF()

SET(EMBREE_LIBRARY_FILES_AVX2

    geometry/instance_intersector1.cpp
    geometry/grid_soa.cpp
    ../common/subdiv/subdivpatch1base_eval.cpp

    bvh/bvh_intersector1.cpp)

IF (RTCORE_RAY_PACKETS)
  SET(EMBREE_LIBRARY_FILES_AVX2 ${EMBREE_LIBRARY_FILES_AVX2}
    geometry/instance_intersector.cpp

    bvh/bvh_intersector_single.cpp
    bvh/bvh_intersector_hybrid.cpp)
ENDIF()

IF (TARGET_AVX2 AND EMBREE_LIBRARY_FILES_AVX2)
 ADD_DEFINITIONS(-D__TARGET_AVX2__)
ENDIF()

SET(EMBREE_LIBRARY_FILES_AVX512

    geometry/instance_intersector1.cpp
    geometry/grid_soa.cpp
    ../common/subdiv/subdivpatch1base_eval.cpp

    builders/primrefgen.avx512.cpp
    bvh/bvh_builder.avx512.cpp
    bvh/bvh_builder_sah.avx512.cpp

    bvh4/bvh4_refit.cpp
    bvh4/bvh4_builder_subdiv.avx512.cpp
    bvh/bvh_intersector1.cpp)

IF (RTCORE_RAY_PACKETS)
  SET(EMBREE_LIBRARY_FILES_AVX512 ${EMBREE_LIBRARY_FILES_AVX512}

    geometry/instance_intersector.cpp

    bvh/bvh_intersector_single.cpp
    bvh/bvh_intersector_hybrid.cpp)
ENDIF()

IF (TARGET_AVX512 AND EMBREE_LIBRARY_FILES_AVX512)
 ADD_DEFINITIONS(-D__TARGET_AVX512__)
ENDIF()

ADD_ISPC_LIBRARY(embree ${EMBREE_LIB_TYPE} ${EMBREE_LIBRARY_FILES})
SET_PROPERTY(TARGET embree PROPERTY FOLDER kernels)
SET_TARGET_PROPERTIES(embree PROPERTIES COMPILE_FLAGS "-DDONT_WARN_INCLUDE_EMBREE_INTERNALS=1")

IF (TARGET_SSE42 AND EMBREE_LIBRARY_FILES_SSE42)
  ADD_LIBRARY(embree_sse42 STATIC ${EMBREE_LIBRARY_FILES_SSE42})
  SET_TARGET_PROPERTIES(embree_sse42 PROPERTIES COMPILE_FLAGS "${FLAGS_SSE42} -DDONT_WARN_INCLUDE_EMBREE_INTERNALS=1")
  SET_PROPERTY(TARGET embree_sse42 PROPERTY FOLDER kernels)
  SET(EMBREE_LIBRARIES ${EMBREE_LIBRARIES} embree_sse42)
ENDIF ()

IF (TARGET_AVX  AND EMBREE_LIBRARY_FILES_AVX)
  ADD_LIBRARY(embree_avx STATIC ${EMBREE_LIBRARY_FILES_AVX})
  SET_TARGET_PROPERTIES(embree_avx PROPERTIES COMPILE_FLAGS "${FLAGS_AVX} -DDONT_WARN_INCLUDE_EMBREE_INTERNALS=1")
  SET_PROPERTY(TARGET embree_avx PROPERTY FOLDER kernels)
  SET(EMBREE_LIBRARIES ${EMBREE_LIBRARIES} embree_avx)
ENDIF()

IF (TARGET_AVX2 AND EMBREE_LIBRARY_FILES_AVX2)
  ADD_LIBRARY(embree_avx2 STATIC ${EMBREE_LIBRARY_FILES_AVX2})
  SET_TARGET_PROPERTIES(embree_avx2 PROPERTIES COMPILE_FLAGS "${FLAGS_AVX2} -DDONT_WARN_INCLUDE_EMBREE_INTERNALS=1")
  SET_PROPERTY(TARGET embree_avx2 PROPERTY FOLDER kernels)
  SET(EMBREE_LIBRARIES ${EMBREE_LIBRARIES} embree_avx2)
ENDIF()

IF (TARGET_AVX512 AND EMBREE_LIBRARY_FILES_AVX512)
  ADD_LIBRARY(embree_avx512 STATIC ${EMBREE_LIBRARY_FILES_AVX512})
  SET_TARGET_PROPERTIES(embree_avx512 PROPERTIES COMPILE_FLAGS "${FLAGS_AVX512} -DDONT_WARN_INCLUDE_EMBREE_INTERNALS=1")
  SET_PROPERTY(TARGET embree_avx512 PROPERTY FOLDER kernels)
  SET(EMBREE_LIBRARIES ${EMBREE_LIBRARIES} embree_avx512)
ENDIF()

TARGET_LINK_LIBRARIES(embree ${EMBREE_LIBRARIES} sys simd lexers)

IF (OSPRAY_ZIP_MODE)
  SET_TARGET_PROPERTIES(embree PROPERTIES VERSION ${EMBREE_VERSION_MAJOR} SOVERSION ${EMBREE_VERSION_MAJOR})
ELSE()
  SET_TARGET_PROPERTIES(embree PROPERTIES VERSION ${EMBREE_VERSION} SOVERSION ${EMBREE_VERSION_MAJOR})
ENDIF()

IF (WIN32)
  INSTALL(TARGETS embree
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} COMPONENT lib
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT devel
  )
ELSE()
  INSTALL(TARGETS embree LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT lib NAMELINK_SKIP)
ENDIF()


#SET_TARGET_PROPERTIES(embree PROPERTIES COMPILE_FLAGS " -DDONT_WARN_INCLUDE_EMBREE_INTERNALS=1")
