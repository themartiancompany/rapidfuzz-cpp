# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>
# Contributor: Pekka Ristola <pekkarr [at] protonmail [dot] com>

_os="$( \
  uname \
    -o)"
pkgname=rapidfuzz-cpp
pkgver=3.0.2
pkgrel=1
_pkgdesc=(
  'Rapid fuzzy string matching'
  'in C++ using the Levenshtein Distance'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  # 'any' it doesn't seem 'any'
  x86_64
  arm
  armv7l
  mips
  powerpc
  i686
  aarch64
)
_http="https://github.com"
_ns="maxbachmann"
url="${_http}/${_ns}/${pkgname}"
license=(
  'MIT'
)
depends=(
  'cmake'
)
makedepends=(
  'catch2'
)
source=(
  "${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/tags/v${pkgver}.tar.gz"
)
b2sums=(
  '49743ab1634649bf5a2b33d91dce1877b0c3135345d9c187eb2d633a1bb804701697d8245a74cfde2a76564abeb3fc8228dce121dcc1a05f74630400d06bb659'
)

prepare() {
  if [[ "${_os}" == "Android" ]]; then
    echo \
      "add_cxx_compiler_flag(-Wno-shorten-64-to-32)" >> \
      "${srcdir}/CMakeLists.txt"
    echo \
      "add_cxx_compiler_flag(-Wno-deprecated-declarations)" >> \
      "${srcdir}/CMakeLists.txt"
  fi
}

build() {
  local \
    _cmake_opts=() \
    _cxxflags=()
  _cxxflags=(
    "${CFLAGS}"
    "${CXXFLAGS}"
  )
  _cmake_opts=(
    -D RAPIDFUZZ_BUILD_TESTING=ON 
    -D CMAKE_BUILD_TYPE=None 
    -D CMAKE_INSTALL_PREFIX=/usr 
    -W no-dev
  )
  if [[ "${_os}" == "GNU/Linux" ]]; then
    _cmake_opts+=(
      -D RAPIDFUZZ_ENABLE_LINTERS=ON 
    )
  elif [[ "${_os}" == "Android" ]]; then
    _cmake_opts+=(
      -D RAPIDFUZZ_BUILD_TESTING=OFF
      -W no-shorten-64-to-32
      -W no-deprecated-declarations
    )
    _cxxflags+=(
      -Wno-error=shorten-64-to-32
      -Wno-error=deprecated-declarations
    )
  fi
  echo \
    "CXX flags: ${_cxxflags[*]}"
  export \
    CXXFLAGS="${_cxxflags[*]}"
  CXXFLAGS="${_cxxflags[*]}" \
  cmake \
    -B build \
    -S "${pkgname}-${pkgver}" \
    "${_cmake_opts[@]}"
  CXXFLAGS="${_cxxflags[*]}" \
  cmake \
    --build \
    build
}

check() {
  ctest \
    --test-dir \
      build \
    --output-on-failure
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
    build

  # license
  install \
    -vDm644 \
    -t \
    "$pkgdir/usr/share/licenses/$pkgname" \
    "$pkgname-$pkgver/LICENSE"
}

