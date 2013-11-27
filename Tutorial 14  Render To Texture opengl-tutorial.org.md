��ʮ�ĿΣ���Ⱦ������
===
[TOC]

����Ⱦ��������һϵ����Ч����֮һ������˼���ǣ���ͨ��������Ⱦһ����������ֻ���������Ⱦ���������õ������С�

Ӧ�ð�������Ϸ��in-game����������ڴ���post-processing���Լ���������һ��.

��Ⱦ������
---
�������������񣺴���Ҫ��Ⱦ��������󣻽�������Ⱦ�������ϣ�ʹ�����ɵ�����

###������ȾĿ�꣨Render Target��###

����Ҫ��Ⱦ�Ķ������֡���档����һ�������������������һ����ѡ����Ȼ�����(depth buffer)����OpenGL�����ǿ����񴴽���������һ��������:
```cpp
// The framebuffer, which regroups 0, 1, or more textures, and 0 or 1 depth buffer.
GLuint FramebufferName = 0;
glGenFramebuffers(1, &amp;FramebufferName);
glBindFramebuffer(GL_FRAMEBUFFER, FramebufferName);
```
������Ҫ�������������а�����ɫ����RGB�������δ���ǳ��ľ��䣺
```cpp
// The texture we're going to render to
GLuint renderedTexture;
glGenTextures(1, &amp;renderedTexture);

// "Bind" the newly created texture : all future texture functions will modify this texture
glBindTexture(GL_TEXTURE_2D, renderedTexture);

// Give an empty image to OpenGL ( the last &quot;0&quot; )
glTexImage2D(GL_TEXTURE_2D, 0,GL_RGB, 1024, 768, 0,GL_RGB, GL_UNSIGNED_BYTE, 0);

// Poor filtering. Needed !
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_NEAREST);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
```
ͬʱ����Ҫһ����Ȼ�������depth buffer�������ǿ�ѡ�ģ�ȡ����������ʵ����Ҫ���Ķ���������������Ⱦ����С��Suzanne��������Ҫ��Ȳ��ԡ�
```cpp
// The depth buffer
GLuint depthrenderbuffer;
glGenRenderbuffers(1, &amp;depthrenderbuffer);
glBindRenderbuffer(GL_RENDERBUFFER, depthrenderbuffer);
glRenderbufferStorage(GL_RENDERBUFFER, GL_DEPTH_COMPONENT, 1024, 768);
glFramebufferRenderbuffer(GL_FRAMEBUFFER, GL_DEPTH_ATTACHMENT, GL_RENDERBUFFER, depthrenderbuffer);
```
�������frameBuffer��
```cpp
// Set "renderedTexture" as our colour attachement #0
glFramebufferTexture(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, renderedTexture, 0);

// Set the list of draw buffers.
GLenum DrawBuffers[2] = {GL_COLOR_ATTACHMENT0};
glDrawBuffers(1, DrawBuffers); // "1" is the size of DrawBuffers
```
��������п��ܳ���һЩ����ȡ����GPU�����ܣ������Ǽ��ķ�����
```cpp
// Always check that our framebuffer is ok
if(glCheckFramebufferStatus(GL_FRAMEBUFFER) != GL_FRAMEBUFFER_COMPLETE)
    return false;
```
###��Ⱦ������###

��Ⱦ�������ֱ�ۡ��򵥵ذ�֡���棬Ȼ��������һ�������������ɸ㶨��
```cpp
// Render to our framebuffer
glBindFramebuffer(GL_FRAMEBUFFER, FramebufferName);
glViewport(0,0,1024,768); // Render on the whole framebuffer, complete from the lower left corner to the upper right
```
fragment shaderֻ������������
```glsl
layout(location = 0) out vec3 color;
```
����ζ��ÿ���޸ı�����color��ʱ��ʵ���޸���0����ȾĿ�ꣻ������Ϊ֮ǰ������`glFramebufferTexture(GL_FRAMEBUFFER, GL_COLOR_ATTACHMENT0, renderedTexture, 0);

ע�⣺���һ��������ʾmipmap�ļ������0��GL_COLOR_ATTACHMENT0û���κι�ϵ��

###ʹ����Ⱦ��������###
���ǽ���һ���򵥵�������Ļ���ı��Ρ���Ҫbuffer��shader��ID����
```cpp
// The fullscreen quad's FBO
GLuint quad_VertexArrayID;
glGenVertexArrays(1, &quad_VertexArrayID);
glBindVertexArray(quad_VertexArrayID);

static const GLfloat g_quad_vertex_buffer_data[] = {
    -1.0f, -1.0f, 0.0f,
    1.0f, -1.0f, 0.0f,
    -1.0f,  1.0f, 0.0f,
    -1.0f,  1.0f, 0.0f,
    1.0f, -1.0f, 0.0f,
    1.0f,  1.0f, 0.0f,
};

GLuint quad_vertexbuffer;
glGenBuffers(1, &quad_vertexbuffer);
glBindBuffer(GL_ARRAY_BUFFER, quad_vertexbuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(g_quad_vertex_buffer_data), g_quad_vertex_buffer_data, GL_STATIC_DRAW);

// Create and compile our GLSL program from the shaders
GLuint quad_programID = LoadShaders( "Passthrough.vertexshader", "SimpleTexture.fragmentshader" );
GLuint texID = glGetUniformLocation(quad_programID, "renderedTexture");
GLuint timeID = glGetUniformLocation(quad_programID, "time");
```
��������Ⱦ����Ļ�ϵĻ��������glBindFramebuffer�ĵڶ���������Ϊ0��
```cpp
// Render to the screen
glBindFramebuffer(GL_FRAMEBUFFER, 0);
glViewport(0,0,1024,768); // Render on the whole framebuffer, complete from the lower left corner to the upper right
```
�������������shader����ȫ�����ı��Σ�
```glsl
#version 330 core

in vec2 UV;

out vec3 color;

uniform sampler2D renderedTexture;
uniform float time;

void main(){
    color = texture( renderedTexture, UV + 0.005*vec2( sin(time+1024.0*UV.x),cos(time+768.0*UV.y)) ).xyz;
}
``` 

��δ���ֻ�Ǽ򵥵ز�����������һ����ʱ��仯��΢Сƫ�ơ�

���
---
<img class="alignnone size-large wp-image-326" title="wavvy" src="http://www.opengl-tutorial.org/wp-content/uploads/2011/05/wavvy-1024x793.png" alt="" width="640" height="495" />

��һ��̽��
---
###ʹ�����###
��һЩ����£�ʹ������Ⱦ�����������Ҫ��ȡ������У��������������򵥵���Ⱦ�������У�
```cpp
glTexImage2D(GL_TEXTURE_2D, 0,GL_DEPTH_COMPONENT24, 1024, 768, 0,GL_DEPTH_COMPONENT, GL_FLOAT, 0);
```
(��24���Ǿ��ȡ�����԰����16,24,32��ѡ��ͨ��24�պ�)

������Щ�Ѿ��㹻�����ˡ��γ�Դ������������ʵ�֡�

���п����е�������Ϊ�����޷�ʹ��[Hi-Z](http://developer.amd.com/media/gpu_assets/Depth_in-depth.pdf)�����Ż���

��ͼ����Ȳ���Ѿ������ֶ����Ż�����ͨ���������������ô��������������У��� = Z�ӽ�0 = ��ɫ� Զ = Z�ӽ�1 = ��ɫǳ��

<img class="alignnone size-large wp-image-337" title="wavvydepth" src="http://www.opengl-tutorial.org/wp-content/uploads/2011/05/wavvydepth-1024x793.png" alt="" width="640" height="495" />

###���ز���###
�ܹ��ö��ز��������������������ֻ��Ҫ��C++�����н�glTexImage2D�滻Ϊ[glTexImage2DMultisample](http://www.opengl.org/sdk/docs/man3/xhtml/glTexImage2DMultisample.xml)����fragment shader�н�`sampler2D/texture`�滻Ϊ`sampler2DMS/texelFetch`�� 

��Ҫע�⣺`texelFetch`�����һ����������ʾ���������������仰˵������û���Զ����˲������ڶ��ز����У���ȷ�������ǡ��ֱ��ʣ�resolution���������ܡ�

������Ҫ���Լ�������ز������������⣬�Ƕ��ز��������Ƕ����һ����ɫ����

û��ʲô�ѵ㣬ֻ������Ӵ�

###������ȾĿ��###
�������Ҫͬʱд�������

�򵥵ش�������������Ҫ����ȷ��һ�µĴ�С����������glFramebufferTexture��Ϊÿһ����������һ����ͬ��color attachement���ø��µĲ�������`(2,{GL_COLOR_ATTACHMENT0,GL_COLOR_ATTACHMENT1,GL_DEPTH_ATTACHMENT})`һ��������glDrawBuffers��Ȼ����Ƭ����ɫ���ж����һ�����������
```glsl
layout(location = 1) out vec3 normal_tangentspace; // or whatever
```
��ʾ1���������Ҫ�������������������������Ҳ���еģ�������16��32λ���ȴ���8λ��������[glTexImage2D](http://www.opengl.org/sdk/docs/man/xhtml/glTexImage2D.xml)�Ĳο��ֲᣨ��GL_FLOAT����
��ʾ2��������ǰ�汾��OpenGL����ʹ��glFragData[1] = myvalue��

��ϰ
---
- ��ʹ��`glViewport(0,0,512,768)`����`glViewport(0,0,1024,768)`����֡���桢��Ļ������������ԣ�
- �����һ��fragment shader�г���һ��������UV����
- ����һ�������ı任����任�ı��Ρ�������Ӳ���뷽ʽ��Ȼ����ʹ��`controls.hpp`����ĺ������۲쵽��ʲô����

> &copy; http://www.opengl-tutorial.org/

> Written with [StackEdit](https://stackedit.io/).