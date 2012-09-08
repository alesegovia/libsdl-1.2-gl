libsdl-1.2-gl
=============

A fork of the libsdl-1.2.15 library that adds support for creating OpenGL Core Profile contexts on OS X.

Rationale
=========

Mac OS X 10.8 (Mountain Lion) implements OpenGL 3.2 core profile. The way to access these features, however, is by creating a forward-compatible-only OpenGL Context when the application starts.

Some popular libraries, like SDL in its version 1.2, do not include the ability to request the Operating System to create forward-compatible OpenGL profiles. The next major release of SDL (libsdl 2.0) will address this problem, however, some toolkits built on SDL 1.2 will not transition instantly to 2.0.

This project adds SDL 1.2 the ability to create OpenGL Core profiles under Mac OS X. The aim for the future is to add support for other platforms such as Windows and Linux (through GLX).

Configure on Mac OS X
=====================

Modern Mac OS X versions do not ship with X, therefore you might want to configure SDL to be built without X.

In order to do this, first create a directory where the build files are to be created (for instance "build"), then cd into that directory and run configure with the following options:

../configure --prefix=<build destination> --disable-video-x11 --disable-x11-shared --without-x

Build and Install
=================

Once configured, just type make && make install to install to your specified build destination.


Usage
=====

The new SDL_OPENGLCORE macro has been defined to create an OpenGL Core Profile context. The following program creates a window with a backing OpenGL Core Profile context, prints the OpenGL Version and quits:

```
#include <SDL.h>
#include <stdio.h>
#include <OpenGL/gl.h>

int main(int argc, char* argv[])
{
	SDL_Init(SDL_INIT_VIDEO);

	SDL_Surface* pSurface = SDL_SetVideoMode(600, 600, 32, SDL_DOUBLEBUF|SDL_OPENGL|SDL_OPENGLCORE);

	printf("GL Version:%s\n", glGetString(GL_VERSION));

	return 0;
}
```
Note: remember to compile and link against your recently built SDL and not the standard 1.2 SDL.

If SDL was installed correctly and your system supports OpenGL 3.2, that is the version that should be printed by this program.

If you omit the ```SDL_OPENGLCORE``` flag, SDL will by default create a legacy context, and the OpenGL version reported will be 2.1.

Caveats
=======

The SDK for Mac OS X 10.8 is required in order to build this library with OpenGL Core Profile Context support. Older SDKs do not include the flags needed to request such profile to be created.

If this case is detected, a #warning message will be printed while compiling the library and all OpenGL contexts created will be legacy, even if the SDL_OPENGLCORE flag is specified.

License
=======

This library is distributed under a GNU LGPL v2 license. libsdl is property of the SDL team. Visit their site at libsdl.org.

Warranty
========

This library is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU Library General Public License for more details.

