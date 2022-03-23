### visual studio 中显示cg，hlsl语法高亮
 download the [CG Toolkit](http://developer.nvidia.com/object/cg_toolkit.html) from the nVidia site.
    
Then in the folder /NVIDIA Corporation/Cg/msdev_syntax_highlighting/ its gonna be a file calledusertype.datthen you will have to do this..

1. Copy the usertype.dat file to

32位： /Microsoft Visual Studio 8/Common7/IDE

64位下是\Program Files (x86)\Microsoft Visual Studio 8\Common7\IDE\这里


2. Open up the registry editor and go to the following location -

32位： HKLM/SOFTWARE/Microsoft/VisualStudio/8.0/Languages/File Extensions

64位：HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\VisualStudio\8.0\Languages\File Extensions


3. Copy the default value from the .cpp key.
4. Create a new key under the File Extensions with the name of .cg（.fx 或者其他什么名字）
5. Paste the value you just copied into the default value.
6. Restart Visual Studio and your shaders should now have syntax highlighting.

后面部分不修改注册表也可以的，在tool->option->text editor->file extention添加fx,hlsl,cg解释为c++语法高亮就可以了。