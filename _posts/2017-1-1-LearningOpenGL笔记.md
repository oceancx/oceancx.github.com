---
---

2.1 Core-profile vs Immediate mode

Q:why do I want to learn OpenGL 3.3 when OpenGL 4.5 is out?

A:As of today, much higher versions of OpenGL are published (at the time of writing 4.5) at
which you might ask: why do I want to learn OpenGL 3.3 when OpenGL 4.5 is out? The answer to
that question is relatively simple. All future versions of OpenGL starting from 3.3 basically add
extra useful features to OpenGL without changing OpenGL’s core mechanics; the newer versions
just introduce slightly more efficient or more useful ways to accomplish the same tasks. The result
is that all concepts and techniques remain the same over the modern OpenGL versions so it is
perfectly valid to learn OpenGL 3.3. Whenever you’re ready and/or more experienced you can
easily use specific functionality from more recent OpenGL versions.

2.3 State machine

OpenGL is by itself a large state machine: a collection of variables that define how OpenGL should
currently operate. The state of OpenGL is commonly referred to as the OpenGL context. When
using OpenGL, we often change its state by setting some options, manipulating some buffers and
then render using the current context.

2.4 Objects

The OpenGL libraries are written in C and allows for many derivations in other languages, but in
its core it remains a C-library. Since many of C’s language-constructs do not translate that well to
other higher-level languages, OpenGL was developed with several abstractions in mind. One of
those abstractions are objects in OpenGL.

3. Creating a window

3.1 GLFW http://www.glfw.org/		glfw3.lib opengl32.lib（windows）
		
		#include <GLFW\glfw3.h>

3.2.1 CMake http://www.cmake.org/cmake/resources/software.html
	

3.5.1 Building and linking GLEW  （glew32s.lib）

	#define GLEW_STATIC
	#include <GL/glew.h>

Static linking of a library means that during compilation the library will be integrated in
your binary file. This has the advantage that you do not need to keep track of extra files,
but only need to release your single binary. The disadvantage is that your executable
becomes larger and when a library has an updated version you need to re-compile your
entire application.
Dynamic linking of a library is done via .dll files or .so files and then the library code
and your binary code stays separated, making your binary smaller and updates easier.
The disadvantage is that you’d have to release your DLLs with the final application.

4. Hello Window

		// GLEW
		#define GLEW_STATIC
		#include <GL/glew.h>
		// GLFW
		#include <GLFW/glfw3.h>
		#include <iostream>
		#include "../Graphics/Shader.h"

		void key_callback(GLFWwindow* window, int key, int scancode, int action,
			int mode);


		int main()
		{
			glfwInit();
			glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
			glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
			glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
			glfwWindowHint(GLFW_RESIZABLE, GL_FALSE);

			GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", nullptr,
				nullptr);
			if (window == nullptr)
			{
				std::cout << "Failed to create GLFW window" << std::endl;
				glfwTerminate();
				return -1;
			}
			glfwMakeContextCurrent(window);

			glewExperimental = GL_TRUE;
			if (glewInit() != GLEW_OK)
			{
				std::cout << "Failed to initialize GLEW" << std::endl;
				return -1;
			}

			glfwSetKeyCallback(window, key_callback);

			glClearColor(0.7f, 0.3f, 0.3f, 1.0f);

			while (!glfwWindowShouldClose(window))
			{
				glfwPollEvents();

				glClear(GL_COLOR_BUFFER_BIT);

				glfwSwapBuffers(window);
			}
			glfwTerminate();

			return 0;
		}


		void key_callback(GLFWwindow* window, int key, int scancode, int action,
			int mode)
		{
			// When a user presses the escape key, we set the WindowShouldClose property to true,
			// closing the application
			if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
				glfwSetWindowShouldClose(window, GL_TRUE);
		}

4.1 GLEW

In the previous tutorial we mentioned that GLEW manages function pointers for OpenGL so we
want to initialize GLEW before we call any OpenGL functions.

		glewExperimental = GL_TRUE;

Notice that we set the glewExperimental variable to GL_TRUE before initializing GLEW.
Setting glewExperimental to true ensures GLEW uses more modern techniques for managing
OpenGL functionality. Leaving it to its default value of GL_FALSE might give issues when using
the core profile of OpenGL.

4.2 Viewport

glViewport(0, 0, 800, 600);
The first two parameters set the location of the lower left corner of the window. The third
and fourth parameter set the width and height of the rendering window, which is the same as the
GLFW window. We could actually set this at values smaller than the GLFW dimensions; then all
the OpenGL rendering would be displayed in a smaller window and we could for example display
other elements outside the OpenGL viewport.


Behind the scenes OpenGL uses the data specified via glViewport to transform the 2D
coordinates it processed to coordinates on your screen. For example, a processed point of
location (-0.5,0.5) would (as its final transformation) be mapped to (200,450)
in screen coordinates. Note that processed coordinates in OpenGL are between -1 and 1
so we effectively map from the range (-1 to 1) to (0, 800) and (0, 600).


The glfwWindowShouldClose function checks at the start of each loop iteration if GLFW
has been instructed to close, if so, the function returns true and the game loop stops running, after
which we can close the application.


The glfwPollEvents function checks if any events are triggered (like keyboard input or
mouse movement events) and calls the corresponding functions (which we can set via callback
methods). We usually call eventprocessing functions at the start of a loop iteration.

The glfwSwapBuffers will swap the color buffer (a large buffer that contains color values
for each pixel in GLFW’s window) that has been used to draw in during this iteration and show it
as output to the screen.

4.6 Rendering

As you might recall from the OpenGL tutorial, the glClearColor function is a statesetting
function and glClear is a state-using function in that it uses the current state
to retrieve the clearing color from.

5. Hello Triangle

The graphics pipeline can be divided into two large parts: the first transforms your
3D coordinates into 2D coordinates and the second part transforms the 2D coordinates into actual
colored pixels.

>>>There is a difference between a 2D coordinate and a pixel. A 2D coordinate is a very precise representation of where a point is in 2D space, while a 2D pixel is an approximation of that point limited by the resolution of your screen/window.

![graphics pipeline](p36)

The first part of the pipeline is the vertex shader that takes as input a single vertex. The main
purpose of the vertex shader is to transform 3D coordinates into different 3D coordinates (more on
that later) and the vertex shader allows us to do some basic processing on the vertex attributes.

The primitive assembly stage takes as input all the vertices (or vertex if GL_POINTS is chosen)
from the vertex shader that form a primitive and assembles all the point(s) in the primitive shape
given; in this case a triangle.

The output of the primitive assembly stage is passed to the geometry shader. The geometry
shader takes as input a collection of vertices that form a primitive and has the ability to generate
other shapes by emitting new vertices to form new (or other) primitive(s). In this example case, it
generates a second triangle out of the given shape.

The tessellation shaders have the ability to subdivide the given primitive into many smaller
primitives. This allows you to for example create much smoother environments by creating more
triangles the smaller the distance to the player.

The output of the tessellation shaders is then passed on to the rasterization stage where it maps
the resulting primitive to the corresponding pixels on the final screen, resulting in fragments for the
fragment shader to use. Before the fragment shaders runs, clipping is performed. Clipping discards
any fragments that are outside your view, increasing performance.

The main purpose of the fragment shader is to calculate the final color of a pixel and this
is usually the stage where all the advanced OpenGL effects occur. Usually the fragment shader
contains data about the 3D scene that it can use to calculate the final pixel color (like lights, shadows,
color of the light and so on).

After all the corresponding color values have been determined, the final object will then pass
through one more stage that we call the alpha test and blending stage. This stage checks the
corresponding depth (and stencil) value (we’ll get to those later) of the fragment and uses those
to check if the resulting fragment is in front or behind other objects and should be discarded
accordingly. The stage also checks for alpha values (alpha values define the opacity of an object)
and blends the objects accordingly. So even if a pixel output color is calculated in the fragment
shader, the final pixel color could still be something entirely different when rendering multiple
triangles.


Normalized Device Coordinates (NDC)

Once your vertex coordinates have been processed in the vertex shader, they should
be in normalized device coordinates which is a small space where the x, y and z
values vary from -1.0 to 1.0.

Unlike usual screen coordinates the positive y-axis points in the up-direction and the
(0,0) coordinates are at the center of the graph, instead of top-left. Eventually you
want all the (transformed) coordinates to end up in this coordinate space, otherwise they
won’t be visible.

Your NDC coordinates will then be transformed to screen-space coordinates via the
viewport transform using the data you provided with glViewport. The resulting
screen-space coordinates are then transformed to fragments as inputs to your fragment
shader.

5.2 Vertex shader

#version 330 core
layout (location = 0) in vec3 position;
void main()
{
	gl_Position = vec4(position.x, position.y, position.z, 1.0);
}


5.6 Element Buffer Objects

GLfloat vertices[] = {
0.5f, 0.5f, 0.0f, // Top Right
0.5f, -0.5f, 0.0f, // Bottom Right
-0.5f, -0.5f, 0.0f, // Bottom Left
-0.5f, 0.5f, 0.0f // Top Left
};
GLuint indices[] = { // Note that we start from 0!
0, 1, 3, // First Triangle
1, 2, 3 // Second Triangle
};


GLuint EBO;
glGenBuffers(1, &EBO);

glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices,
GL_STATIC_DRAW);

glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);


7. Textures

7.1 Texture Wrapping
 GL_REPEAT: The default behavior for textures. Repeats the texture image.
 GL_MIRRORED_REPEAT: Same as GL_REPEAT but mirrors the image with each repeat.
 GL_CLAMP_TO_EDGE: Clamps the coordinates between 0 and 1. The result is that higher
coordinates become clamped to the edge, resulting in a stretched edge pattern.
 GL_CLAMP_TO_BORDER: Coordinates outside the range are now given a user-specified
border color.


7.7 Texture Units

You probably wondered why the sampler2D variable is a uniform if we didn’t even assign it
some value with glUniform. Using glUniform1i we can actually assign a location value to
the texture sampler so we can set multiple textures at once in a fragment shader. This location of a
texture is more commonly known as a texture unit.The default texture unit for a texture is 0 which
is the default active texture unit so we did not had to assign a location in the previous section.



So to fix our little issue we have 2 options:
1. We can alter the texture coordinates in the vertex data and swap the y values (subtract the y
coordinates from 1).
2. We can edit the vertex shader to swap the y-coordinate automatically by replacing the TexCoord
assignment with TexCoord = vec2(texCoord.x, 1.0f - texCoord.y);.

8.6 Vector-vector multiplication

8.6.1 Dot product

The dot product of two vectors is equal to the scalar product of their lengths times the cosine of the
angle between them.

v*k = |v|*|k|*cosθ
the cosine or cos function becomes 0 when the angle is 90 degrees or 1 when the angle is 0.

This
allows us to easily test if the two vectors are orthogonal or parallel to each other using the dot
product (orthogonal means the vectors are at a right-angle to each other).

8.6.2 Cross product

The cross product is only defined in 3D space and takes two non-parallel vectors as input and
produces a third vector that is orthogonal to both the input vectors.

8.7 Matrices
8.16 Combining matrices

The true power from using matrices for transformations is that we can combine multiple transformations
in a single matrix thanks to matrix-matrix multiplication.

It is advised to first do scaling operations, then rotations and lastly
translations when combining matrices otherwise they might (negatively) affect each other.

The advantage of transforming them to several
intermediate coordinate systems is that some operations/calculations are easier in certain coordinate
systems as will soon become apparent.

1. Local space (or Object space)
2. World space
3. View space (or Eye space)
4. Clip space
5. Screen space

As you can see, due to perspective the lines seem to coincide the farther they’re away. This is
exactly the effect perspective projection tries to mimic and it does so using a perspective projection
matrix.

distorted pulled or twisted out of shape; contorted.


9.7 Going 3D
Right-handed system
By convention, OpenGL is a right-handed system.

Next we need to create a view matrix. We want to move slightly backwards in the scene so
the object becomes visible (when in world space we’re located at the origin (0,0,0)). To move
around the scene, think about the following:
 To move a camera backwards, is the same as moving the entire scene forward.
That is exactly what a view matrix does, we move the entire scene around inversed to where we
want the camera to move.

Because we want to move backwards and since OpenGL is a right-handed system we have to
move in the positive z-axis. We do this by translating the scene towards the negative z-axis. This
gives the impression that we are moving backwards.

To understand why it’s called right-handed do the following:
 Stretch your right-arm along the positive y-axis with your hand up top.
 Let your thumb point to the right.
 Let your pointing finger point up.
 Now bend your middle finger downwards 90 degrees.

10.6 Euler angles
The pitch is the angle that depicts how much we’re looking up or down as seen in the first
image. The second image shows the yaw value which represents the magnitude we’re looking to
the left or to the right. The roll represents how much we roll as mostly used in space-flight cameras.
Each of the Euler angles are represented by a single value and with the combination of all 3 of them
we can calculate any rotation vector in 3D.

hypotenuse

soh cah toa = just so litter

10.7 Mouse input

before eventually retrieving the direction vector:
1. Calculate the mouse’s offset since the last frame.
2. Add the offset values to the camera’s yaw and pitch values.
3. Add some constraints to the maximum/minimum yaw/pitch values
4. Calculate the direction vector

12. Colors

absorbed
perceive 感知

