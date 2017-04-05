How to build the toolchain
--------------------------

Please note that these instructions were updated using 32-bit Ubuntu Desktop 
16.04LTS as the working environment.

We are using GNU ARM as the toolchain. It is important to build your own 
version of the toolchain, because we require libgcc to be compiled with the 
mthumb-interwork option. The rest of our project is also built with 
mthumb-interwork, and it is nice to be able to make use of libgcc's integer 
arithmetic library.

Here is how we build our toolchain, and some of the problems we ran into (and 
their solutions). We can't guarantee you won't run into your own unique 
problems, however.

## Step one: Grab the sources

We use the following sources linked from their respective websites for the 
GCC 6.3 toolchain. Use the following URLs:

- binutils: http://ftp.gnu.org/gnu/binutils/binutils-2.28.tar.bz2
- gcc: http://ftp.gnu.org/gnu/gcc-6.3.0/gcc-6.3.0.tar.bz2
- newlib: ftp://sourceware.org/pub/newlib/newlib-2.5.0.20170323.tar.gz

You can just (I'm going to give code examples optimized for copy and pasting 
rather than flexibility) :

	wget http://ftp.gnu.org/gnu/binutils/binutils-2.28.tar.bz2 
	wget http://ftp.gnu.org/gnu/gcc-6.3.0/gcc-6.3.0.tar.bz2
	wget ftp://sourceware.org/pub/newlib/newlib-2.5.0.20170323.tar.gz

	tar jxvf binutils-2.28.tar.bz2
	tar jxvf gcc-6.3.0.tar.bz2
	tar zxvf newlib-2.5.0.20170323.tar.gz

## Step two: Build and install binutils

This step is fairly straightforward.

Please note that specifying the prefix is **not optional** in some of these 
steps (there are problems with configuring GCC without an explicit prefix), 
so go ahead and just specify it for all configures. We use /usr/local, 
because that's the default location and already is in the path.

	mkdir binutils-build
	cd binutils-build
	../binutils-2.28/configure --target=arm-none-eabi --prefix=/usr/local \
		--enable-interwork --enable-multilib --disable-werror
	make -j2 all
	sudo make install
	cd ..

## Step three: Build and install first stage of GCC

Now you can configure and build. Note that the configure (and thus build) 
must be run as root because configure wants to copy some headers into 
/usr/local. Annoying.

	mkdir gcc-build
	cd gcc-build
	sudo ../gcc-6.3.0/configure --target=arm-none-eabi --prefix=/usr/local \
		--enable-interwork --enable-multilib \
		--enable-languages="c,c++" --with-newlib \
		--with-headers=../newlib-2.5.0.20170323/newlib/libc/include
	sudo make -j2 all-gcc
	sudo make install-gcc
	cd ..

## Step four: Build and install newlib

Pretty easy after the GCC step.

	mkdir newlib-build
	cd newlib-build
	../newlib-2.5.0.20170323/configure --target=arm-none-eabi --prefix=/usr/local \
		--enable-interwork --enable-multilib

If you don't have makeinfo installed, install GNU's texinfo. 
Now, you should be able to build.

	make -j2 all
	sudo make install
	cd ..

## Step five: Building the rest of GCC

	cd gcc-build
	sudo make -j2 all
	sudo make install

And you're done.
