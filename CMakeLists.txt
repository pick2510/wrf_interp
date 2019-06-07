cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(wrf_interp Fortran)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

find_package(OpenMP)
if (OPENMP_FOUND)
	set (CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} ${OpenMP_Fortran_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()


set (NETCDF_F90 "YES")
find_package (NetCDF REQUIRED)
include_directories(${NETCDF_INCLUDES})

set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -Wall")
set(CMAKE_Fortran_FLAGS_DEBUG "-g -Wall -Wextra -Warray-temporaries -Wconversion -fimplicit-none -fbacktrace -ffree-line-length-0 -fcheck=all -ffpe-trap=zero,overflow,underflow -finit-real=nan")
set(CMAKE_Fortran_FLAGS_RELEASE "-O3 -mtune=native -fopt-info")

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/namelist.vinterp
        DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(GLOB_RECURSE sources  *.F90)
add_executable(wrf_interp ${sources})
target_link_libraries(wrf_interp  ${NETCDF_LIBRARIES})


