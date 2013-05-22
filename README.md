this version is known to compile against v8 3.15.12
here are the approximate steps:

if [ `/bin/arch -m` == 'i86pc' ] ; then
	## solaris will not compile, you'll need to use the original 0.07 version of Javascript up on CPAN from DGL
	##	then use the libcsw libv8 packages
elif [ `/bin/uname -m` == 'x64_64' ] ; then
	## 64 bit
   make x64.release library=shared soname_version=3.15.12
   /bin/cp -v out/x64.release/obj.target/tools/gyp/* /usr/local/lib
   /bin/cp -v include/* /usr/local/include/
   /bin/ln -fs /usr/local/lib/libv8.so.3.15.12 /usr/local/lib/libv8.so
   ldconfig
elsif [ `/bin/uname -m` == 'i686' ] ; then
	## 32 bit
   make ia32.release library=shared soname_version=3.15.12
   /bin/cp -v out/ia32.release/obj.target/tools/gyp/* /usr/local/lib
   /bin/cp -v include/* /usr/local/include/
   /bin/ln -fs /usr/local/lib/libv8.so.3.15.12 /usr/local/lib/libv8.so
   ldconfig
fi

/bin/cp -v include/*.h /usr/local/include
export V8_DIR=/usr/local/src/asdf/v8-3.15.12/

perl Makefile.PL
make test
