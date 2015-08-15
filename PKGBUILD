_pkgname=conky
pkgname=conky-lua51-git
pkgver=2.0
pkgrel=1
pkgdesc='Lightweight system monitor for X'
url='http://conky.sourceforge.net/'
license=('BSD' 'GPL')
arch=('i686' 'x86_64')
makedepends=('pkg-config' 'cmake' 'docbook2x' 'git')
depends=('alsa-lib' 'libxml2' 'curl' 'wireless_tools' 'libxft' 'libxdamage' 'imlib2' 'lua51' 'tolua++')
conflicts=('conky')
provides=('conky')
source=('git://github.com/brndnmtthws/conky')
md5sums=('SKIP')

pkgver() {
	cd $_pkgname
	git describe --always | sed -e 's|-|.|g'
}

build() {
	cd "${srcdir}/${_pkgname}"
        # CPPFLAGS="${CXXFLAGS}" LIBS="${LDFLAGS}" LUA_LIBS="-llua5.1" LUA_CFLAGS="-I/usr/include/lua5.1"
	sed -i  's/lua5.2 lua-5.2 lua>=5.1//g' ../conky/cmake/ConkyPlatformChecks.cmake
	cmake -D CMAKE_INSTALL_PREFIX:PATH=/usr \
	-D CMAKE_BUILD_TYPE:STRING="Release" \
	-D MAINTAINER_MODE:BOOL=ON \
	-D BUILD_CURL:BOOL=ON \
	-D BUILD_I18N:BOOL=ON \
	-D BUILD_RSS:BOOL=ON \
	-D BUILD_WEATHER_XOAP:BOOL=ON \
	-D BUILD_WEATHER_METAR:BOOL=ON \
	-D BUILD_IMLIB2:BOOL=ON \
	-D BUILD_LUA_IMLIB2:BOOL=ON \
	-D BUILD_LUA_CAIRO:BOOL=ON \
	-D BUILD_MYSQL:BOOL=OFF \
	-D BUILD_WLAN:BOOL=ON \
	../conky

	make
}

package() {
	cd "${srcdir}/${_pkgname}"
	make DESTDIR="${pkgdir}" install
	install -d "${pkgdir}/usr/share/licenses/${_pkgname}"
	install -m644 ../conky/{COPYING,LICENSE}* "${pkgdir}/usr/share/licenses/${_pkgname}"
	install -Dm644 extras/vim/syntax/conkyrc.vim "${pkgdir}"/usr/share/vim/vimfiles/syntax/conkyrc.vim
	install -Dm644 extras/vim/ftdetect/conkyrc.vim "${pkgdir}"/usr/share/vim/vimfiles/ftdetect/conkyrc.vim
	install -d "${pkgdir}/etc/${_pkgname}"
	install -Dm644 "${pkgdir}"/usr/share/doc/conky-2.0.0_pre/conky{,_no_x11}.conf "${pkgdir}"/etc/conky
}
