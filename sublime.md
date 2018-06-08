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
    
  **快捷键**
  
Ctrl+D 选中光标所占的文本，继续操作则会选中下一个相同的文本。

Alt+F3 选中文本按下快捷键，即可一次性选择全部的相同文本进行同时编辑。举个栗子：快速选中并更改所有相同的变量名、函数名等。

Ctrl+L 选中整行，继续操作则继续选择下一行，效果和 Shift+↓ 效果一样。

Ctrl+Shift+L 先选中多行，再按下快捷键，会在每行行尾插入光标，即可同时编辑这些行。

Ctrl+Shift+M 选择括号内的内容（继续选择父括号）。举个栗子：快速选中删除函数中的代码，重写函数体代码或重写括号内里的内容。

Ctrl+M 光标移动至括号内结束或开始的位置。

Ctrl+Enter 在下一行插入新行。举个栗子：即使光标不在行尾，也能快速向下插入一行。

Ctrl+Shift+Enter 在上一行插入新行。举个栗子：即使光标不在行首，也能快速向上插入一行。

Ctrl+Shift+[ 选中代码，按下快捷键，折叠代码。

Ctrl+Shift+] 选中代码，按下快捷键，展开代码。

Ctrl+K+0 展开所有折叠代码。

Ctrl+← 向左单位性地移动光标，快速移动光标。

Ctrl+→ 向右单位性地移动光标，快速移动光标。

shift+↑ 向上选中多行。

shift+↓ 向下选中多行。

Shift+← 向左选中文本。

Shift+→ 向右选中文本。

Ctrl+Shift+← 向左单位性地选中文本。

Ctrl+Shift+→ 向右单位性地选中文本。

Ctrl+Shift+↑ 将光标所在行和上一行代码互换（将光标所在行插入到上一行之前）。

Ctrl+Shift+↓ 将光标所在行和下一行代码互换（将光标所在行插入到下一行之后）。

Ctrl+Alt+↑ 向上添加多行光标，可同时编辑多行。

Ctrl+Alt+↓ 向下添加多行光标，可同时编辑多行。

编辑类

Ctrl+J 合并选中的多行代码为一行。举个栗子：将多行格式的CSS属性合并为一行。

Ctrl+Shift+D 复制光标所在整行，插入到下一行。

Tab 向右缩进。

Shift+Tab 向左缩进。

Ctrl+K+K 从光标处开始删除代码至行尾。

Ctrl+Shift+K 删除整行。

Ctrl+/ 注释单行。

Ctrl+Shift+/ 注释多行。

Ctrl+K+U 转换大写。

Ctrl+K+L 转换小写。

Ctrl+Z 撤销。

Ctrl+Y 恢复撤销。

Ctrl+U 软撤销，感觉和 Gtrl+Z 一样。

Ctrl+F2 设置书签

Ctrl+T 左右字母互换。

F6 单词检测拼写

搜索类

Ctrl+F 打开底部搜索框，查找关键字。

Ctrl+shift+F 在文件夹内查找，与普通编辑器不同的地方是sublime允许添加多个文件夹进行查找，略高端，未研究。

Ctrl+P 打开搜索框。举个栗子：1、输入当前项目中的文件名，快速搜索文件，2、输入@和关键字，查找文件中函数名，3、输入：和数字，跳转到文件中该行代码，4、输入#和关键字，查找变量名。

Ctrl+G 打开搜索框，自动带：，输入数字跳转到该行代码。举个栗子：在页面代码比较长的文件中快速定位。

Ctrl+R 打开搜索框，自动带@，输入关键字，查找文件中的函数名。举个栗子：在函数较多的页面快速查找某个函数。

Ctrl+： 打开搜索框，自动带#，输入关键字，查找文件中的变量名、属性名等。

Ctrl+Shift+P 打开命令框。场景栗子：打开命名框，输入关键字，调用sublime text或插件的功能，例如使用package安装插件。

Esc 退出光标多行选择，退出搜索框，命令框等。

显示类

Ctrl+Tab 按文件浏览过的顺序，切换当前窗口的标签页。

Ctrl+PageDown 向左切换当前窗口的标签页。

Ctrl+PageUp 向右切换当前窗口的标签页。

Alt+Shift+1 窗口分屏，恢复默认1屏（非小键盘的数字）

Alt+Shift+2 左右分屏-2列

Alt+Shift+3 左右分屏-3列

Alt+Shift+4 左右分屏-4列

Alt+Shift+5 等分4屏

Alt+Shift+8 垂直分屏-2屏

Alt+Shift+9 垂直分屏-3屏

Ctrl+K+B 开启/关闭侧边栏。

F11 全屏模式

Shift+F11 免打扰模式
