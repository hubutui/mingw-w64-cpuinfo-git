# Maintainer: Hu Butui <hot123tea123@gmail.com>

_realname=cpuinfo
pkgbase=mingw-w64-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}-git")
pkgver=r615.728f3e9
pkgrel=1
pkgdesc="A common bricks library for building scalable and portable distributed machine learning"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32')
url="https://github.com/pytorch/cpuinfo"
license=('BSD')
makedepends=(
  "${MINGW_PACKAGE_PREFIX}-cmake"
  git
)
source=("${_realname}::git+https://github.com/pytorch/cpuinfo.git")
sha256sums=('SKIP')
pkgver() {
  cd "${_realname}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}
build() {
  echo "==> Build static version"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -B "${srcdir}/build-static-${MINGW_CHOST}" \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DCPUINFO_BUILD_BENCHMARKS=OFF \
    -DCPUINFO_BUILD_MOCK_TESTS=OFF \
    -DCPUINFO_BUILD_UNIT_TESTS=OFF \
    -DCPUINFO_LIBRARY_TYPE=static \
    -G'MSYS Makefiles' \
    -S ${_realname}
  ${MINGW_PREFIX}/bin/cmake.exe --build "${srcdir}/build-static-${MINGW_CHOST}"

  echo "==> Build shared version"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -B "${srcdir}/build-shared-${MINGW_CHOST}" \
    -DBUILD_SHARED_LIBS=ON \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DCPUINFO_BUILD_BENCHMARKS=OFF \
    -DCPUINFO_BUILD_MOCK_TESTS=OFF \
    -DCPUINFO_BUILD_UNIT_TESTS=OFF \
    -DCPUINFO_LIBRARY_TYPE=shared \
    -G'MSYS Makefiles' \
    -S ${_realname}
  ${MINGW_PREFIX}/bin/cmake.exe --build "${srcdir}/build-shared-${MINGW_CHOST}"
}

package() {
  DESTDIR="${pkgdir}" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    --build "${srcdir}/build-static-${MINGW_CHOST}" \
    --target install

  DESTDIR="${pkgdir}" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    --build "${srcdir}/build-shared-${MINGW_CHOST}" \
    --target install
}
