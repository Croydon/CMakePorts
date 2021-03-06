
include(ExternalProject)

if (APPLE)
  include (InstallMacPorts.cmake)
endif (APPLE)

##____________________________________________________________________
##                                                     Port categories

foreach (category_
    archivers
    database
    devel
    editors
    graphics
    lang
    math
    multimedia
    net
    office
    parallel
    print
    science
    sysutils
    tex
    textproc
    www
    x11
    )
    add_subdirectory (${category_})
endforeach(category_)

##____________________________________________________________________
##                                                Package dependencies

add_dependencies (cloog polylib ppl)
#add_dependencies (gdl gsl hdf5 plplot)
#add_dependencies (mathgl gif)
#add_dependencies (pnetlib qt)
#add_dependencies (heasoft cfitsio)
#add_dependencies (isis slang cfitsio heasoft)
#add_dependencies (xstar ncurses termcap)

## ___ devel _________________

add_dependencies (apr-util apr)
add_dependencies (bison help2man)
add_dependencies (cppunit autoconf automake)
add_dependencies (doxygen swig)
#add_dependencies (gcc mpc cloog libiconv)
add_dependencies (git subversion curl expat openssl zlib)
add_dependencies (libmpc mpfr)
add_dependencies (libtool automake)
add_dependencies (mpfr gmp)
add_dependencies (ppl gmp)
add_dependencies (subversion swig expat neon serf sqlite)

## ___ editors ___________________________________

add_dependencies (nedit lesstif)
add_dependencies (nano ncurses)

## ___ graphics __________________________________

add_dependencies (paraview treecc)

## ___ lang ______________________________________

add_dependencies (guile gmp)
add_dependencies (python readline gettext zlib)

## ___ multimedia ________________________________

add_dependencies (libvorbis libogg)
add_dependencies (libvpx yasm)
add_dependencies (libmatroska libebml)
add_dependencies (mplayer libvpx)
#add_dependencies (vlc libdvdcss libdvb libvorbis a52dec freetype)

## ___ net _______________________________________

add_dependencies (curl openssl)

## ___ science ___________________________________

add_dependencies (casacore casacore_measures hdf5 wcslib cfitsio bison blas)
add_dependencies (dal casacore doxygen)
add_dependencies (duchamp wcslib cfitsio)
add_dependencies (hdf5 openmpi szip)
add_dependencies (netcdf hdf5 curl)
add_dependencies (pelican qt cppunit)
#add_dependencies (pelican-lofar pelican)
add_dependencies (silo hdf5)

## ___ www _______________________________________

add_dependencies (neon openssl)
add_dependencies (serf apr apr-util openssl)

##____________________________________________________________________
##                                            Collective build targets

## ___ Daily updates/builds ______________________

add_custom_target (Daily ALL
  WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
)

add_dependencies (Daily
  cmake
  doxygen
  mplayer
  root
  casacore
  )

## ___ Weekly updates/builds _____________________

add_custom_target (Weekly ALL
  WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
)

add_dependencies (Weekly
  llvm
  paraview
  cloudy
  mplayer
  )
