ubuntu 16.04 install sublime-text_3

    sudo add-apt-repository ppa:webupd8team/sublime-text-3
    sudo apt update
    sudo apt install sublime-text-installer

install packege control

    ctrl + ` 

    //copy the code 
    
    import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)  
    
install sublime plugins
    
    //install c/c++ plugin
    sudo apt-get install cmake build-essential clang git
    cd ~/.config/sublime-text-3/Packages
    git clone --recursive https://github.com/quarnster/SublimeClang SublimeClang
    cd SublimeClang
    cp /usr/lib/x86_64-linux-gnu/libclang-3.4.so.1 internals/libclang.so
