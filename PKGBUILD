pkgname=mono3
pkgver=3.0.3
pkgrel=1
pkgdesc="Free implementation of the .NET platform including runtime and 
compiler"

url="http://www.mono-project.com/"
arch=('i686' 'x86_64')

license=('GPL' 'LGPL2' 'MPL' 'custom:MITX11')
depends=('zlib' 'libgdiplus>=2.10' 'sh')
makedepends=('pkgconfig')
options=('!libtool' '!makeflags')
provides=('monodoc' 'mono=3.0.3')
conflicts=('monodoc' 'mono' 'mono-git')
md5sums=('c1e9fb125f620597a9bc1cdc1fee9288')
source=("http://download.mono-project.com/sources/mono/mono-${pkgver}.tar.bz2")

build() 
{
   	cd ${srcdir}/mono-${pkgver}

	# commit 2646f8b18289e4ef5fe8193ed376dd475af786f6
   	sed -i -e "s:AM_CONFIG_HEADER([config.h]):AC_CONFIG_HEADERS([config.h]):" ./configure.in ./eglib/configure.ac
	sed -i -e "s:AUTOMAKE_OPTIONS = cygnus:# AUTOMAKE_OPTIONS = cygnus:" ./runtime/Makefile.am
	sed -i -e "s:AM_PROG_CC_STDC:# AM_PROG_CC_STDC:" ./configure.in

   	#build mono
   	./configure --prefix=/usr --sysconfdir=/etc \
                --with-libgdiplus=installed
   	make V=1 || return 1
}

package() {
  	cd ${srcdir}/mono-${pkgver}
  	make DESTDIR=${pkgdir} install || return 1

  	#install license
  	mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  	install -m644 mcs/MIT.X11 ${pkgdir}/usr/share/licenses/${pkgname}/

  	#fix the .pc file to be able to request mono on what it depends, fixes #go-oo build
  	sed -i -e "s:#Requires:Requires:" ${pkgdir}/usr/lib/pkgconfig/mono.pc
}
