# compile mex
cmake_minimum_required(VERSION 2.8)

set(PLAYREC_SOURCE_FILES  
   pa_dll_playrec.c 
   mex_dll_core.c 
  )  
set(PLAYREC_HEADER_FILES
   pa_dll_playrec.h 
   mex_dll_core.h
  )

if (PLAYREC_TARGET MATCHES Octave)
     set(PLAYREC_INCLUDE_DIRS ${OCTAVE_INCLUDE_DIRS})
     set(PLAYREC_LIBRARY_DIRS ${OCTAVE_LIBRARY_DIRS})
     set(PLAYREC_LIBRARIES ${OCTAVE_LIBRARIES})
     set(PLAYREC_MEX_SUFFIX ${OCTAVE_MEX_SUFFIX})
     set(PLAYREC_COMP_FLAGS ${OCTAVE_COMP_FLAGS})
     set(PLAYREC_LINK_FLAGS ${OCTAVE_LINK_FLAGS})     
else (PLAYREC_TARGET MATCHES Octave)
     set(PLAYREC_INCLUDE_DIRS ${MATLAB_INCLUDE_DIRS})
     set(PLAYREC_LIBRARY_DIRS ${MATLAB_LIBRARY_DIRS})
     set(PLAYREC_LIBRARIES ${MATLAB_LIBRARIES})
     set(PLAYREC_MEX_SUFFIX ${MATLAB_MEX_SUFFIX})
     set(PLAYREC_COMP_FLAGS ${MATLAB_COMP_FLAGS})
     set(PLAYREC_LINK_FLAGS ${MATLAB_LINK_FLAGS})
endif (PLAYREC_TARGET MATCHES Octave)

include_directories(${PLAYREC_INCLUDE_DIRS} ${PORTAUDIO_INCLUDE_DIRS})
link_directories(${PLAYREC_LIBRARY_DIRS})

add_library(playrec SHARED ${PLAYREC_SOURCE_FILES})
target_link_libraries(playrec ${PLAYREC_LIBRARIES} ${PORTAUDIO_LIBRARIES})
set_target_properties(playrec PROPERTIES SUFFIX ${PLAYREC_MEX_SUFFIX} 
                                         PREFIX "") 

if (NOT ${PLAYREC_COMP_FLAGS} MATCHES "")
  set_target_properties(playrec PROPERTIES COMP_FLAGS ${PLAYREC_COMP_FLAGS}) 
endif (NOT ${PLAYREC_COMP_FLAGS} MATCHES "")
if (PLAYREC_LINK_FLAGS)
  set_target_properties(playrec PROPERTIES LINK_FLAGS ${PLAYREC_LINK_FLAGS}) 
endif (PLAYREC_LINK_FLAGS)

install(TARGETS playrec DESTINATION .)
#INSTALL(FILES playrec.m DESTINATION .)
