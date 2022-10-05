PHP 5.4
=======

PHP build steps taken from [https://wiki.php.net/internals/windows/stepbystepbuild](https://wiki.php.net/internals/windows/stepbystepbuild)

## PHP + php_cairo

#### 01)
Install Windows7 32bit


#### 02)
Install Visual Studio 2012 from the "bin\" dirctory.


#### 05)
Open "Developer Command Prompt for VS2012"

    cd C:\php_gtk_builder\php-sdk\
    bin\phpsdk_setvars.bat


#### 08)
download php_cairo source (I used 
[https://github.com/gtkforphp/cairo](https://github.com/gtkforphp/cairo) 
commit: df45aa1418) and extract to `c:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\ext` and rename 
extracted directory to `cairo`

#### 09)
extract `gtk+-bundle_2.24.10-20120208_win32.zip` in `C:\php_gtk_builder` (download from 
[http://ftp.gnome.org/pub/gnome/binaries/win32/gtk+/2.24/](http://ftp.gnome.org/pub/gnome/binaries/win32/gtk+/2.24/))

#### 10)
Copy overwriting existing files:

    C:\php_gtk_builder\gtk\include\cairo               to  C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\include\cairo
    C:\php_gtk_builder\gtk\include\fontconfig          to  C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\include\fontconfig
    C:\php_gtk_builder\gtk\include\freetype2\freetype  to  C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\include\include\freetype
    C:\php_gtk_builder\gtk\include\ft2build.h          to  C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\include\ft2build.h

copy the following files from `C:\php_gtk_builder\gtk\lib\`  into  `C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\lib\`

    cairo.def
    cairo.lib
    fontconfig.def
    fontconfig.lib
    freetype.def
    freetype.lib
    libcairo.dll.a
    libfontconfig.dll.a
    libfreetype.dll.a
 
#### 11)
Open `C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\include\fontconfig\fontconfig.h`
with a text-editor. Find the line that says:

    #include <unistd.h>

and replace with:
    
    #ifdef PHP_WIN32
    # include "win32/unistd.h"
    #else
    # include <unistd.h>
    #endif    

#### 12)
php printer

#### 12)
run in the windows-sdk-shell:

    cd c:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src
    buildconf --force && 
    configure --with-mp=8 --with-gd --enable-cli --disable-zts --enable-cli-win32 --with-cairo=shared --disable-cgi --with-mysqli --disable-ipv6 --disable-phar --with-dom --with-mcrypt --enable-soap --with-openssl --enable-sockets --enable-mbstring --enable-printer --with-curl  --disable-calendar --disable-com-dotnet --disable-ctype  --disable-filter --disable-ftp --without-t1lib --without-libvpx --disable-mbregex-backtrack  --disable-odbc  --disable-session    --disable-tokenizer    --without-wddx   --disable-xmlwriter   --disable-xmlreader   --without-xml --enable-debug --enable-crt-debug

    

    nmake

#### 13)
Copy `php_cairo.dll` from `C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release` to
    `C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release\ext`

copy the following files from `C:\php_gtk_builder\gtk\bin` to `C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release`

    freetype6.dll
    libcairo-2.dll
    libexpat-1.dll
    libfontconfig-1.dll
    libpng14-14.dll
    zlib1.dll
       
#### 13)
Copy `ssleay32.dll`,`libssh2.dll`  and `libeay32.dll` from `C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\bin` to
    `C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release`


#### 14)
Create a `php.ini` in `C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release` with 
the following content:

    extension_dir=.\ext
    extension=php_cairo.dll
    extension=php_curl.dll

#### 15)
test the newly built PHP

    cd C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release
    php -v
    php -m


## php_gtk2


#### 16)
get `grep` and `sed` for windows (for example: 
[http://unxutils.sourceforge.net/UnxUtils.zip](http://unxutils.sourceforge.net/UnxUtils.zip))
and put them in the `PATH` (ex. `c:\windows`)

#### 17)
extract php-gtk sources to `C:\php_gtk_builder\php-gtk` (I used 
[https://github.com/auroraeosrose/php-gtk-src](https://github.com/auroraeosrose/php-gtk-src)
commit: e972b2524a)
                
#### 18)
We need a `php.exe`, so add `C:\php_gtk_builder\php-sdk\php56\vc11\x86\Release` to 
`PATH` in `"C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\Tools"\vsvars32.bat`

#### 19)
In  `"C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\Tools\vsvars32.bat"`

    add to INCLUDE  C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src
    add to INCLUDE  C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\include
    add to LIB      C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\Release
    add to LIB      C:\php_gtk_builder\php-sdk\php56\vc11\x86\deps\lib

#### 21)
extract
[http://www.php.net/extra/win32build.zip](http://www.php.net/extra/win32build.zip)
into `C:\php_gtk_builder\win32build`
                              
#### 22)
copy from `C:\php_gtk_builder\gtk` to `C:\php_gtk_builder\win32build`

    include\cairo                     -> include\cairo
    include\gtk-2.0\*                 -> include\*
    include\glib-2.0\*                -> include\*
    include\atk-1.0\atk               -> include\atk
    include\pango-1.0\pango           -> include\pango
    include\gdk-pixbuf-2.0\gdk-pixbuf -> include\gdk-pixbuf
    include\fontconfig                -> include\fontconfig
    include\freetype2\freetype        -> include\freetype

    lib\*  -> lib\*

Overwrite conflicting files

#### 23)
copy 

    c:\php_build\lib\glib-2.0\include\*   -> c:\php_build\include\*
    c:\php_build\lib\gtk-2.0\include\*    -> c:\php_build\include\*

#### 24)
In  `C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\Tools\vsvars32.bat`

    add to INCLUDE  C:\php_build\include
    add to LIB      C:\php_build\lib
and execute the batch, in the windows sdk shell, to activate the changes


#### 25) ***** HACK *****
copy `C:\Program Files\Microsoft SDKs\Windows\v6.1\Samples\winui\TSF\tsfapp\winres.h`
to `c:\php_build\include`

#### 26)
copy `php_cairo_api.h` and  `php_cairo.h` 
from `C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\ext\cairo` to `C:\php_gtk_builder\win32build\include`

#### 27)
run in the windows-sdk-shell:

    cd C:\php_gtk_builder\php-gtk
    buildconf
    configure --with-php-build=..\win32build --disable-zts --enable-gd
    nmake

#### 28)
copy `C:\php_gtk_builder\php-gtk\Release\php_gtk2.dll` to
`C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\Release\ext\php_gtk2.dll`

copy the following files from `C:\php_gtk_builder\gtk\bin`
to `C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\Release`

    libgtk-win32-2.0-0.dll
    libgdk-win32-2.0-0.dll
    libgdk_pixbuf-2.0-0.dll
    intl.dll
    libgio-2.0-0.dll
    libglib-2.0-0.dll
    libgmodule-2.0-0.dll
    libgobject-2.0-0.dll
    libgthread-2.0-0.dll
    libpango-1.0-0.dll
    libpangocairo-1.0-0.dll
    libpangoft2-1.0-0.dll
    libpangowin32-1.0-0.dll
    libatk-1.0-0.dll

#### 29)
add the following line to `C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\Release\php.ini`

    extension=php_gtk2.dll 

#### 30)
verify that the extension is loaded

    cd C:\php_gtk_builder\php-sdk\php56\vc11\x86\php-src\Release
    php -m


## DONE!