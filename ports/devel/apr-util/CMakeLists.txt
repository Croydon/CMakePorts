
## ==============================================================================
##
##  CMake project settings
##
## ==============================================================================

## Name of the project handled by CMake
project (APR_util)

## Minimum required version of CMake to configure the project
cmake_minimum_required (VERSION 2.8)

## Enforced CMake policy 
cmake_policy (VERSION 2.8)

##  Package information
set (APR_util_VERSION        "1.3.11" )
set (APR_util_SOURCE_ARCHIVE "apr-util-${APR_util_VERSION}.tar.gz" )
set (APR_util_URL            "http://mirror.hostfuss.com/apache/apr/${APR_util_SOURCE_ARCHIVE}" )
set (APR_util_SVN            "" )

## ==============================================================================
##
##  Options
##
## ==============================================================================

option (APR_util_FORCE_BUILD    "Force build and installation of package?" NO )
option (APR_util_FROM_REPOS     "Install package from repository?"         NO )
option (APR_util_ENABLE_TESTING "Enable testing and experimental builds?"  YES )

## Handle options

if (APR_util_ENABLE_TESTING)
  include (CTest)
  enable_testing()
endif (APR_util_ENABLE_TESTING)

## ==============================================================================
##
##  System inspection
##
## ==============================================================================

#include (FindAPR_util)

## ==============================================================================
##
##  Build instructions
##
## ==============================================================================

if (APR_util_FROM_REPOS)
  set (APR_util_FORCE_BUILD TRUE)
endif (APR_util_FROM_REPOS)

if (NOT HAVE_APR_util OR APR_util_FORCE_BUILD)
  
  ## Locate source file to build from
  
  find_file (APR_util_SOURCE ${APR_util_SOURCE_ARCHIVE}
    PATHS ${PROJECT_SOURCE_DIR}
    )
  
  if (NOT APR_util_SOURCE)
    set (APR_util_SOURCE ${APR_util_URL})
  endif (NOT APR_util_SOURCE)
  
  ## Build package from source
  
  ExternalProject_Add (apr-util
    PREFIX ${PROJECT_BINARY_DIR}
    DOWNLOAD_DIR download
    SOURCE_DIR source
    URL ${APR_util_SOURCE}
    BUILD_IN_SOURCE 1
    CONFIGURE_COMMAND ${PROJECT_BINARY_DIR}/source/configure --prefix=${CMAKE_INSTALL_PREFIX} --with-apr=${CMAKE_INSTALL_PREFIX}
    INSTALL_COMMAND sudo make install
    )
  
else (NOT HAVE_APR_util OR APR_util_FORCE_BUILD)
  
  add_custom_target (apr-util
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
    COMMENT "[APR_util] Found system-wide installation; skipping rebuild!"
    )
  
endif (NOT HAVE_APR_util OR APR_util_FORCE_BUILD)