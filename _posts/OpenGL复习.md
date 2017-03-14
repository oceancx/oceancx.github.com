---
---

OpenGL is mainly
considered an API (an Application Programming Interface) that provides us with a large set of
functions that we can use to manipulate graphics and images.


2.1 Core-profile vs Immediate mode

2.3 State machine

OpenGL is by itself a large state machine: a collection of variables that define how OpenGL should
currently operate. The state of OpenGL is commonly referred to as the OpenGL context. When
using OpenGL, we often change its state by setting some options, manipulating some buffers and
then render using the current context.

When working in OpenGL we will come across several state-changing functions that change
the context and several state-using functions that perform some operations based on the current
state of OpenGL. As long as you keep in mind that OpenGL is basically one large state machine,
most of its functionality will make more sense.

2.4 Objects


26.Framebuffers

So far we’ve used several types of screen buffers: a color buffer for writing color values, a depth
buffer to write depth information and finally a stencil buffer that allows us to discard certain
fragments based on some condition. The combination of these buffers is called a framebuffer and is
stored somewhere in memory. OpenGL gives us the flexibility to define our own framebuffers and
thus define our own color and optionally a depth and stencil buffer.

The rendering operations we’ve done so far were all done on top of the render buffers attached
to the default framebuffer. The default framebuffer is created and configured when you create your
window (GLFW does this for us). By creating our own framebuffer we can get an additional means
to render to.

The application of framebuffers might not immediately make sense, but rendering your scene
to a different framebuffer allows us to create mirrors in a scene or do cool post-processing effects
for example. First we’ll discuss how they actually work and then we’ll use them by implementing
those cool post-processing effects.