**ubuntu 16.04 install sublime-text_3**

    sudo add-apt-repository ppa:webupd8team/sublime-text-3
    sudo apt update
    sudo apt install sublime-text-installer

**install packege control**

    ctrl + ` 

    //copy the code 
    
    import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)  
    
**install sublime plugins**
    
    //install c/c++ plugin
    sudo apt-get install cmake build-essential clang git
    cd ~/.config/sublime-text-3/Packages
    git clone --recursive https://github.com/quarnster/SublimeClang SublimeClang
    cd SublimeClang
    cp /usr/lib/x86_64-linux-gnu/libclang-3.4.so.1 internals/libclang.so

**install sublime markdown插件**
    
    crtl+shirt+p  打开control package
    输入install package 进入package安装
    输入markdown editing 安装editing
    输入markdown previewer 安装previewer
    
**sublime 安装中文输入插件**

    新建libsublime-imfix.c文件
    输入以下代码并保存
``` c
    #include <gtk/gtkimcontext.h>
    void gtk_im_context_set_client_window (GtkIMContext *context,
        GdkWindow    *window)
    {
    GtkIMContextClass *klass;
    g_return_if_fail (GTK_IS_IM_CONTEXT (context));
    klass = GTK_IM_CONTEXT_GET_CLASS (context);
    if (klass->set_client_window)
        klass->set_client_window (context, window);
    g_object_set_data(G_OBJECT(context),"window",window);
    if(!GDK_IS_WINDOW (window))
        return;
    int width = gdk_window_get_width(window);
    int height = gdk_window_get_height(window);
    if(width != 0 && height !=0)
        gtk_im_context_focus_in(context);
    }
```
    编译
    
    gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
    
    若出现gtk错误：
    
    fatal error: gtk/gtk.h: No such file or directory
    
    则执行
    
    sudo apt-get install libgtk2.0-dev
    
    然后将libsublime-imfix.so拷贝到sublime_text所在文件夹
    
    sudo mv libsublime-imfix.so /opt/sublime_text/

    修改启动脚本
    
    在/usr/bin/subl中将
    
    #!/bin/sh
    exec /opt/sublime_text/sublime_text "$@"
    
    修改为
    
    #!/bin/sh
    LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text "$@"
    
    修改快捷方式脚本
    
    在 /usr/share/applications/sublime_text.desktop中
    
    将[Desktop Entry]中的字符串
    
    Exec=/opt/sublime_text/sublime_text %F
    
    修改为
    
    Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text %F"
    
    将[Desktop Action Window]中的字符串
    
    Exec=/opt/sublime_text/sublime_text -n
    
    修改为
    
    Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text -n"
    
    将[Desktop Action Document]中的字符串
    
    Exec=/opt/sublime_text/sublime_text --command new_file
    
    修改为
    
    Exec=bash -c "LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so exec /opt/sublime_text/sublime_text --command new_file"
    
    注意：
    修改时请注意双引号"",否则会导致不能打开带有空格文件名的文件。
    此处仅修改了/usr/share/applications/sublime-text.desktop，但可以正常使用了。
    
    opt/sublime_text/目录下的sublime-text.desktop可以修改，也可不修改。
