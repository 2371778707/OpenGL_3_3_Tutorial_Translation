��ʮ���Σ�OpenGL��չ
===
[TOC]

��չ
---
GPU���������Ÿ��»���һֱ����ߣ�֧����Ⱦ����������κ����ص㡣Ȼ����ԭʼ���ܲ�������Ψһ���ĵġ�NVIDIA, AMD��IntelҲͨ�����ӹ������������ǵ��Կ�������һЩ���ӡ�

###ARB_fragment_program###

ʱ�⵹�ص�2002�꣬��ʱGPU��û��vertex shader��fragment shader�����е�һ�ж�Ӳ������оƬ�С��ⱻ��Ϊ�̶�������ˮ�ߣ�Fixed-Function Pipeline (FFP)������ʱ���µ�OpenGL 1.3��ͬ��Ҳû��API���Դ�����������ʹ����ν�ġ���ɫ��������Ϊ�����������ڡ�����NVIDIA������ʵ�ʴ���������Ⱦ���̣���ȡ�����԰ټƵı�Ǻ�״̬���������`ARB_fragment_program`����������ʱ��û��GLSL���������д�����ĳ���
```
    !!ARBfp1.0 MOV result.color, fragment.color; END
```
����Ҫ��ʽ����OpenGLʹ����Щ���룬����ҪһЩ��δ������OpenGL�е����⺯�����ڽ��н���ǰ���پٸ����ӡ�

###ARB_debug_output###

�ðɣ���Ҳ���˵��ARB_fragment_program̫���ˣ��ҿ϶�����Ҫ��չ�ⶫ��������ʵ�в����µ���չ�ǳ����㡣����һ������ARB_debug_output�����ṩ��һ����������OpenGL 3.3�еģ��������/Ӧ���õ��Ĺ��ܡ�����������GL_DEBUG_OUTPUT_SYNCHRONOUS_ARB��GL_DEBUG_SEVERITY_MEDIUM_ARB֮����ַ�������DebugMessageCallbackARB�����ĺ����������չ��ΰ��֮�����ڣ�����д��һЩ����ȷ�Ĵ��룬���磺
```cpp
glEnable(GL_TEXTURE); // Incorrect ! You probably meant GL_TEXTURE_2D !
```
���ܵõ�������Ϣ�ʹ���ľ�ȷλ�á��ܽ᣺

- ��������ʽ��OpenGL 3.3�У���չ�Ծ�ʮ�����á�
- ��ʹ��ARB_debug_output �����������ӡ�

<img class="alignnone size-large wp-image-622" title="breakpoint" src="http://www.opengl-tutorial.org/wp-content/uploads/2012/02/breakpoint-1024x678.png" alt="" width="640" height="423" />

###��ȡ��չ - ���ӵķ�ʽ ###

���ֶ�������һ����չ�ķ�����ʹ�����´���Ƭ�ϣ�ת��[OpenGL.org wiki](http://www.opengl.org/wiki/GlGetString)����
```cpp
int NumberOfExtensions;
glGetIntegerv(GL_NUM_EXTENSIONS, &amp;NumberOfExtensions);
for(i=0; i&lt;NumberOfExtensions; i++) {
  const GLubyte *ccc=glGetStringi(GL_EXTENSIONS, i);
  if ( strcmp(ccc, (const GLubyte *)&quot;GL_ARB_debug_output&quot;) == 0 ){
    // The extension is supported by our hardware and driver
    // Try to get the &quot;glDebugMessageCallbackARB&quot; function :
    glDebugMessageCallbackARB  = (PFNGLDEBUGMESSAGECALLBACKARBPROC) wglGetProcAddress(&quot;glDebugMessageCallbackARB&quot;);
  }
}
```
###������е���չ - �򵥵ķ�ʽ###

����ķ�ʽ̫���ӡ�����GLEW, GLee, gl3w��Щ�⣬�ͼ򵥶��ˡ����磬����GLEW����ֻ��Ҫ�ڴ������ں����`glewInit()`�����ٷ���ı����ʹ������ˣ�  
```cpp
if (GLEW_ARB_debug_output){ // Ta-Dah ! }
```
��С�ģ�`debug_output`������⣬��Ϊ����Ҫ�ڴ��������ĵ�ʱ������������GLFW�У���ͨ��`glfwOpenWindowHint(GLFW_OPENGL_DEBUG_CONTEXT, 1)`��ɡ���

###ARB vs EXT vs ...###
��չ�����ְ�ʾ���������÷�Χ��

+ GL_������ƽ̨��
+ GLX_����Linux��Mac�¿�ʹ�ã�X11����
+ WGL_����Windows�¿�ʹ�á�
+ EXT��һ����չ��
+ ARB���Ѿ���OpenGL�ܹ�����ίԱ�ᣨOpenGL Architecture Review Board �������г�Ա���ܣ�һ��EXT��չ���þͱ�����ΪARB������չ��
NV/AMD/INTEL������˼�� =)

�������չ
---
###����###

��������OpenGL 3.3Ӧ�ó�����Ҫ��ȾһЩ����������������дһ�����ӵ�vertex shader����ɣ����߼򵥵���[GL_NV_path_rendering](http://www.opengl.org/registry/specs/NV/path_rendering.txt)�����ܰ��㴦�����и��ӵ��¡�

�������������д���룺
```cpp
if ( GLEW_NV_path_rendering ){
    glPathStringNV( ... ); // Draw the shape. Easy !
}else{
    // Else what ? You still have to draw the lines
    // on older NVIDIA hardware, on AMD and on INTEL !
    // So you have to implement it yourself anyway !
}
```
###���⿼��###

��ʹ����չ���洦������Ⱦ���������ܣ�����ά�����ֲ�ͬ������������Ĵ��룬һ�ֿ����Լ�ʵ�֣�һ��ʹ����չ���Ĵ���ʱ��ͨ����ѡ������չ��

���磬��ʱ�ջþ���Braid, һ��ʱ�մ�Խ��2D��Ϸ���У��������ʱ��ʱ���ͻ��и��ָ�����ͼ�����Ч����������Ч���ھ�Ӳ����û����Ⱦ��

����OpenGL 3.3�����߰汾�У�������99%�Ŀ��ܻ��õ��Ĺ��ߡ�һЩ��չ�����ã�����`GL_AMD_pinned_memory`����Ȼ��ͨ��û������ǰʹ��`GL_ARB_framebuffer_object`������������Ⱦ�������������Ϸ���������10����

��������ò�������Ӳ������ô�Ͳ�����OpenGL 3+��������OpenGL 2+�����档����������ʹ�ø����������չ�ˣ��������д�����Щ���⡣

�����ϸ�ڿ��Բο�����[OpenGL 2.1�汾�ĵ�14�� - ������Ⱦ����152��](http://code.google.com/p/opengl-tutorial-org/source/browse/tutorial14_render_to_texture/tutorial14.cpp?name=2.1%20branch#152)�����ֶ����`GL_ARB_framebuffer_object`�Ƿ���ڡ���������ɼ�[FAQ](http://www.opengl-tutorial.org/miscellaneous/faq/)��
����
---
OpenGL��չ�ṩ��һ���ܺõķ�ʽ����ǿOpenGL�Ĺ��ܣ������������û���GPU�� 

��Ȼ������չ���ڸ߼��÷�����Ϊ�󲿷ֹ����ں������Ѿ����ˣ����˽���չ�����������ô�������������ܣ��������ߵ�ά�����ۣ����Ǻ���Ҫ�ġ�

����Ķ�
---

- [debug_output tutorial by Aks](http://sites.google.com/site/opengltutorialsbyaks/introduction-to-opengl-4-1---tutorial-05) ����GLEW��������������һ����
- [The OpenGL extension registry](http://www.opengl.org/registry/) ������չ�Ĺ��˵�������Ǳ����䡣
- [GLEW](http://glew.sourceforge.net/) OpenGL��׼��չ��
- [gl3w](https://github.com/skaslev/gl3w)�򵥵�OpenGL 3/4 core profile���� 


> &copy; http://www.opengl-tutorial.org/

> Written with [StackEdit](https://stackedit.io/).