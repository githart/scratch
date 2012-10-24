
OpenCog
=======

OpenCog is a framework for developing AI systems, especially appropriate
for integrative multi-algorithm systems, and artificial general intelligence
systems.  Though much work remains to be done,
it currently contains a functional core framework, and a number of
cognitive agents at varying levels of completion, some already displaying
interesting and useful functionalities alone and in combination.

Prerequisites
-------------
To build and run OpenCog, the following packages are required. With a
few exceptions, most Linux distributions will have these packages.

Older versions of CMake may not have scripts to find all of these packages,
some needed CMake scripts are included in lib, but others in lib/compat are
provided if you are trying to build with an old version of CMake (although the
best option is to get the latest CMake if possible).

###### boost  
> -- a C++ utilities package -  
> http://www.boost.org/  
> Debian/Ubuntu package libboost-dev

###### cmake  
> -- a build management tool  
> http://www.cmake.org/  
> Debian/Ubuntu package cmake

cxxtest     -- a test framework
               http://cxxtest.sourceforge.net/
               (Not originally available in Debian/Ubuntu; get cxxtest 
               from https://launchpad.net/~dhart/+archive/ppa)

guile       -- Embedded scheme interperter
               http://www.gnu.org/software/guile/guile.html
               Version 1.8.6 or newer is required.
               Debian/Ubuntu package guile-1.8-dev

libgsl      -- The GNU Scientific Library
               Debian/Ubuntu package libgsl0-dev

Optional prereqs
----------------
The following packages are optional. If they are not installed, some
optional parts of OpenCog will not be built.  The CMake command, during
the build, will be more precise as to which parts will not be built.

curl        -- cURL groks URLs
               Optional package
               Required by opencog/ubigraph
               http://curl.haxx.se/
               Debian/Ubuntu package libcurl4-gnutls-dev

expat       -- an XML parsing library - 
               Optional package, needed for embodiment support.
               http://www.jclark.com/xml/expat.html (version 1.2)
               Debian/Ubuntu package libexpat1-dev
               Alternatively Debian/Ubuntu package libxmltok1-dev 
               (supplies expat v1.2) Version 2 seems to work as well.  
               http://expat.sourceforge.net/

HyperTable  -- Distributed storage.
               Optional package, needed only for experimental support.
               http://hypertable.org.
               This requires SIGAR as well. 

MPI         -- Message Passing Interface
               Optional package
               Required for compute-cluster version of MOSES.
               Use either MPICHV2 or OpenMPI

OpenGL      -- Open Graphics Library
               Optional package
               Required by opencog/spatial/MapTool
               info: http://www.opengl.org
               Commonly provided with your video card driver

SDL         -- Simple DirectMedia Layer
               Optional package
               Required by opencog/spatial/MapTool
               http://www.libsdl.org
               Debian/Ubuntu package libsdl1.2-dev

SDL_gfx     -- Simple DirectMedia Layer extension
               Optional package
               Required by opencog/spatial/MapTool
               http://www.ferzkopp.net/joomla/content/view/19/14/
               Debian/Ubuntu package libsdl-gfx1.2-dev

unixODBC    -- Generic SQL Database client access libraries
               Optional package
               Required for the distributed-processing atomspace.
               http://www.unixodbc.org/
               Debian/Ubuntu packages unixodbc-dev

xercesc     -- Apache Xerces-C++ XML Parser
               Optional package
               Requied for embodiment
               Debian/Ubuntu packages libxerces-c28 and libxerces-c2-dev
 
xmlrpc      -- XML-RPC support
               Optional package
               Required by opencog/ubigraph
               http://www.xmlrpc.com
               Debian/Ubuntu package libxmlrpc-c-dev


To build opencog:
-----------------
Peform the following steps at the shell prompt:

   cd to project root dir
   mkdir bin
   cd bin
   cmake -DCMAKE_BUILD_TYPE=Release ..
   make

Libraries will be built into subdirectories within bin, mirroring the
structure of the source directory root. The flag -DCMAKE_BUILD_TYPE=Release
results in binaries that are optimized for for performance; ommitting 
this flag will result in faster builds, but slower executables.


Unit tests:
-----------
To build and run the unit tests, from the ./bin directory enter (after
building opencog as above): 

   make test


Running the server:
-------------------
The cogserver provides a simple server interface to the reasoning
system.

See CommandRequestProcessor.cc as an example control interface to
the server.  This command processor understands 3 simple commands:
load <xml file name>, ls and shutdown. There is an example XML file
under tests/server/atomSpace.xml

To run a simple test, build everything and execute
bin/opencog/server/cogserver. Then, from another terminal, run
"telnet localhost 17001". Try loading the example XML file and ls
to see all the nodes and links.


Config file
-----------
The operation of the server can be altered by means of a config file.
This config file is in lib/opencog.conf. To make use of it, say 
"cogserver -c <config-filename>" when starting the server.


Scheme shell
------------
The cog server also includes a built-in scheme shell. The shell can be
started by typing "scm" after entering the opencog server shell. It can
be exited by placing a single . on a line by itself.  This shell allows
opencog atoms and truth values to be created, manipulated and destroyed
using a very simple but powerful interface.  Examples and documentation
for the available OpenCog commands can be found in src/guile/README.
See also the wiki for additional details.


Modifying the list of basic types
---------------------------------
See the example under ./examples/atomtypes
 

CMake notes
-----------
Some useful CMake's web sites/pages: 

 - http://www.cmake.org (main page) 
 - http://www.cmake.org/Wiki/CMake_Useful_Variables 
 - http://www.cmake.org/Wiki/CMake_Useful_Variables/Get_Variables_From_CMake_Dashboards
 - http://www.cmake.org/Wiki/CMakeMacroAddCxxTest
 - http://www.cmake.org/Wiki/CMake_HowToFindInstalledSoftware


The main CMakeLists.txt currently sets -DNDEBUG. This disables Boost
matrix/vector debugging code and safety checks, with the benefit of
making it much faster. Boost sparse matrixes and (dense) vectors are
currently used by ECAN's ImportanceDiffusionAgent. If you use Boost
ublas in other code, it may be a good idea to at least temporarily
unset NDEBUG. Also if the Boost assert.h is used it will be necessary
to unset NDEBUG. Boost ublas is intended to respond to a specific 
BOOST_UBLAS_NDEBUG, however this is not available as of the current
Ubuntu standard version (1.34).

-Wno-deprecated is currently enabled by default to avoid a number of
warnings regarding hash_map being deprecated (because the alternative
is still experimental!)


