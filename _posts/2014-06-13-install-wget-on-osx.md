Install and configure wget on os x 
----------------------------------

Install and Configure wget on OS X Mavericks 10.9 and fix SSL GNUTLS error
==========================================================================

 

Mac OS X 10.9 Mavericks comes with the command line utility called
‘**curl**‘ which is a network transfer tool, it does not come with the
popular ‘**wget**‘, in fact ‘**curl**‘ can probably get you by just
fine, check *man curl* at the command line to see its usage.

Otherwise let’s look at getting ‘**wget**‘…

To add and install **wget** to your system you need to download the
source files, compile the code and make an install. To actually compile
the code you need a compiler, unfortunately it doesn’t come with OS X by
default you need to install the free xcode suite from Apple which
includes the GCC compiler. This process also works exactly the same in
OSX 10.8 and OSX 10.7.

 

 

Get Xcode
---------

[Get the latest Xcode from the Apple app store, free download
version](http://itunes.apple.com/us/app/xcode/id497799835?ls=1&mt=12 "xcode from apple includes gcc compiler"),
then install and launch it.

Next you need to install the Xcode command line tools, easiest way to do
so by running in the Terminal:

    xcode-select --install

Using curl to get wget
----------------------

[Get the latest wget source code from the ftp
repository](http://ftp.gnu.org/gnu/wget/ "wget latest versions"), or
using curl from the command line:

    cd ~/Downloads

    curl -O http://ftp.gnu.org/gnu/wget/wget-1.14.tar.gz

Extract it and move into it
---------------------------

    tar -zxvf wget-1.14.tar.gz

    cd wget-1.14/

Configure and Install it
------------------------

    ./configure

an error may occur on SSL…

    configure: error: --with-ssl was given, but GNUTLS is not available.

wget needs to have some type of SSL support GNUTLS is most probably not
available on your OS X system – if so use OpenSSL in the configure as an
alternative use so re-run the configure with an SSL flag:

    ./configure --with-ssl=openssl

    make

    sudo make install

Thats it done, wget will be installed in:

    /usr/local/bin/wget

Clean Up
--------

Remove the source code and compressed file:

    rm -rf ~/Downloads/wget*

Test wget

    cd ~/Downloads

    wget http://ftp.gnu.org/gnu/wget/wget-1.14.tar.gz

Everything should work out fine – if you need to install more Unix style
tools it will be faster and better to install a [Package Manager for OSX
like
Homebrew](http://coolestguidesontheplanet.com/setting-up-os-x-mavericks-and-homebrew/ "Installing Homebrew on OS X Mavericks 10.9, Package Manager for Unix Apps") –
it makes installing and maintaining these applications so easy,

 
