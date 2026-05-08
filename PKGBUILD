# SPDX-License-Identifier: AGPL-3.0

#    -----------------------------------------------------
#    Copyright © 2008, 2009, 2010,
#                2011, 2012, 2013,
#                2014, 2015, 2016,
#                2014, 2015, 2016,
#                2017, 2018, 2019,
#                2020, 2021, 2022,
#                2023, 2024, 2025,
#                2026
#                Arch Linux contributors
#    Copyright © 2024, 2025, 2026
#                Pellegrino Prevete
#
#    All rights reserved
#    -----------------------------------------------------
#
#    This program is free software: you can redistribute
#    it and/or modify it under the terms of the
#    GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    This program is distributed in the hope that it
#    will be useful, but WITHOUT ANY WARRANTY;
#    without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU Affero General Public License for
#    more details.
#
#    You should have received a copy of the
#    GNU Affero General Public License
#    along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Giancarlo Razzolini
#     <grazzolini@archlinux.org>
#   Frederik Schwan
#     <freswa at archlinux dot org>
#   Bartłomiej Piotrowski
#     <bpiotrowski@archlinux.org>
#   Allan McRae
#     <allan@archlinux.org>

# toolchain build order:
#   linux-api-headers ->
#     glibc ->
#       binutils ->
#         gcc ->
#           glibc ->
#             binutils ->
#               gcc
# NOTE: valgrind requires rebuilt with each major glibc version

_os="$(
  uname \
    -o)"
_arch="$(
  uname \
    -m)"
_evmfs_available="$(
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
if [[ "${_os}" == "Android" ]]; then
  _compiler="clang"
  _libcompiler="llvm-libs"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler="gcc"
  _libcompiler="gcc-libs"
elif [[ "${_os}" == "Msys" ]]; then
  _compiler="gcc"
  _libcompiler="gcc-libs"
else
  _msg=(
    "Unknown os '${_os}'."
  )
  msg \
    "${_msg[*]}"
  _compiler="gcc"
  _libcompiler="gcc-libs"
fi
if [[ ! -v "_multilib" ]]; then
  if [[ "${_os}" == "GNU/Linux" ]]; then
    if [[ "${_arch}" == "x86_64" ]]; then
      _multilib="true"
    elif [[ "${_arch}" == "i686" ]]; then
      _multilib="false"
    else
      _multilib="false"
    fi
  else
    # Only on GNU x86_64 for now
    _multilib="false"
  fi
fi
if [[ ! -v "_git" ]]; then
  _git="true"
fi
if [[ ! -v "_offline" ]]; then
  _offline="false"
fi
if [[ ! -v "_ns" ]]; then
  _ns="gnu"
  _ns="themartiancompany"
fi
if [[ ! -v "_git_service" ]]; then
  if [[ "${_ns}" == "gnu" ]]; then
    _git_service="sourceware"
  elif [[ "${_ns}" == "themartiancompany" ]]; then
    _git_service="github"
  fi
fi
if [[ ! -v "_git_http" ]]; then
  if [[ "${_git_service}" == "github" ]]; then
    _git_http="https://github.com"
    _git_ns="${_ns}"
  elif [[ "${_git_service}" == "sourceware" ]]; then
    _git_http="https://sourceware.org"
    _git_ns="git"
  fi
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _archive_format="zip"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _archive_format="tar.gz"
    fi
  fi
fi
if [[ "${_os}" == "Android" ]]; then
  _locale="C.UTF-8"
else
  _locale="$(
    locale |
      grep \
        "LANG=" |
        awk \
          -F \
            "=" \
          '{print $1}')"
fi
if [[ ! -v "_en" ]]; then
  _en="true"
  if [[ "${_locale}" == "C.UTF-8" ]]; then
    _en="true"
  fi
fi
if [[ ! -v "_it" ]]; then
  _it="true"
  if [[ "${_locale}" == "it_IT.UTF-8" ]]; then
    _it="true"
  fi
fi
if [[ ! -v "_like" ]]; then
  if [[ "${_it}" == "true" ]]; then
    _like="mo-me-lo-segno"
  fi
  if [[ "${_en}" == "true" ]]; then
    _like="never-gonna-give-you-up"
  fi
fi
if [[ ! -v "_docs" ]]; then
  _docs="true"
fi
_py="python"
_proj=gnu
_pkg=glibc
pkgbase="${_pkg}"
pkgname=(
  "${pkgbase}"
  "${pkgbase}-locales"
)
if [[ "${_multilib}" == "true" ]]; then
  pkgname+=(
    "lib32-${pkgbase}"
  )
fi
pkgver=2.43+r5+g856c426a7534
_commit="856c426a753450b8c6861a5b994a564f4fc16d4b"
_bundle_commit="660c52dc219a6e77f35b5b5d7d0b80e91cc6beb9"
pkgrel=6
arch=(
  "aarch64"
  "arm"
  "armv7l"
  "armv8l"
  "i686"
  "mips"
  "pentium4"
  "powerpc"
  "x86_64"
)
url="https://www.${_proj}.org/software/libc"
license=(
  "GPL-2.0-or-later LGPL-2.1-or-later"
)
makedepends=(
  "${_compiler}"
  "${_libcompiler}"
  "gd"
  "${_py}"
)
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ "${_multilib}" == "true" ]]; then
  makedepends=(
    "lib32-${_libcompiler}"
  )
fi
makedepends_x86_64=(
  "lib32-${_libcompiler}"
)
options=(
  "staticlibs"
  "!lto"
)
if [[ ! -v "_tag_name" ]]; then
  _tag_name="commit"
fi
if [[ ! -v "_tag" ]]; then
  if [[ "${_tag_name}" == "commit" ]]; then
    _tag="${_commit}"
  fi
fi
source=(
  "locale.gen.txt"
  "locale-gen"
  "lib32-${_pkg}.conf"
  "sdt.h"
  "sdt-config.h"
)
sha256sums=(
  "2a7dd6c906b6c54a68f48a21898664a32fdb136cbd9ff7bfd48f01d1aaa649ae"
  "6932c404fcba3e6ded66a459201b6f8eb94c964fd27ea9e0fcafd0433dcc34a8"
  "c27424154a6096ae32c0824b785e05de6acef33d9224fd6147d1936be9b4962b"
  "774061aff612a377714a509918a9e0e0aafce708b87d2d7e06b1bd1f6542fe70"
  "cdc234959c6fdb43f000d3bb7d1080b0103f4080f5e67bcfe8ae1aaf477812f0"
)
_tarname="${_pkg}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_url="${_git_http}/${_git_ns}/${_pkg}"
# source=(
#   "${_tarfile}::git+${_url}.git#commit=${_commit}"
# )
_tag="${_commit}"
_tag_name="commit"
_tarname="${_pkg}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_bundle_tag_name="${_pkg}-${_bundle_commit}..${_commit}"
_bundle_tag_file="${_bundle_tag_name}.${_archive_format}"
if [[ "${_offline}" == "true" ]]; then
  _url="file://${HOME}/${pkgname}"
fi
_sum="SKIP"
_sig_sum="SKIP"
_bundle_sum="32ec977ff219a3c9fa910b373788e7a02c0d09bfb4c888f9acf384b9354d8b1f"
_bundle_sig_sum="aeb8abf4adbb2af8b8bde86653c1003d9d8b3e6182a5fcfeac5591cea6ea66a1"
_bundle_tag_sum="af4509927140a5d80f6267d9ed324b9cb7cdc85274e091dc919578cc10483af2"
_bundle_tag_sig_sum="92bd33cf7613b1abcae8ca0b1debf27fe9b3b19bdb4d6fc5f0f1619b68a2f97b"
_github_sum="bca609d83a88b18e9806ee9f0df6ff7bb2c61c18a134f56321b14fb0939bfd12"
_github_sig_sum="ec42be133e17acb9d759a2a04a1ad82f3b3c835e798afd561a4c275f6921de06"
if [[ "${_git}" == "true" ]]; then
  _sum="${_bundle_sum}"
  _sig_sum="${_bundle_sig_sum}"
elif [[ "${_git}" == "false" ]]; then
  _sum="${_github_sum}"
  _sig_sum="${_github_sig_sum}"
fi
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_bundle_tag_uri="${_evmfs_dir}/${_bundle_tag_sum}"
_bundle_tag_src="${_bundle_tag_file}::${_bundle_tag_uri}"
_sig_uri="${_evmfs_dir}/${_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
_bundle_tag_sig_uri="${_evmfs_dir}/${_bundle_tag_sig_sum}"
_bundle_tag_sig_src="${_bundle_tag_file}.sig::${_bundle_tag_sig_uri}"
if [[ "${_evmfs}" == "true" ]]; then
  _src="${_evmfs_src}"
  source+=(
    "${_sig_src}"
  )
  sha256sums+=(
    "${_sig_sum}"
  )
  if [[ "${_git}" == "true" ]]; then
    source+=(
      "${_bundle_tag_src}"
      "${_bundle_tag_sig_src}"
    )
    sha256sums+=(
      "${_bundle_tag_sum}"
      "${_bundle_tag_sig_sum}"
    )
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}?signed"
    _sum="SKIP"
  elif [[ "${_git}" == false ]]; then
    _uri=""
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/archive/${_commit}.${_archive_format}"
        _sum="${_github_sum}"
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/-/archive/${_tag}/${_tag}.${_archive_format}"
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
source+=(
  "${_src}"
)
sha256sums+=(
  "${_sum}"
)
validpgpkeys=(
  # Carlos O'Donell
  "7273542B39962DF7B299931416792B4EA25340F8"
  # Siddhesh Poyarekar
  "BC7C7372637EC10C57D7AA6579C43DFBF1CF2187"
  # Truocolo
  #   <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
  'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
  # Pellegrino Prevete (dvorak)
  #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
  '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
)

if [[ "${_git}" == "true" ]]; then

pkgver() {
  cd \
    "${_tarname}"
  git \
    describe \
      --abbrev=12 \
      --tags |
    sed \
      's/[^-]*-//;s/[^-]*-/&r/;s/-/+/g'
}

fi

_git_unbundle() {
  local \
    _tarname="${1}" \
    _bundle \
    _repo \
    _msg=()
  _bundle="${srcdir}/${_tarname}.bundle"
  _repo="${srcdir}/${_tarname}"
  _msg=(
    "Cloning '${_bundle}' into '${_repo}'."
  )
  msg \
    "${_msg[*]}"
  git \
    clone \
      "${_bundle}" \
      "${_repo}" || \
  git \
    -C \
      "${_repo}" \
      pull || \
  true
}

_git_unbundle_update() {
  local \
    _repo="${1}" \
    _bundle="${2}" \
    _repo \
    _msg=()
  _bundle_name="$(
    basename \
      "${_bundle}")"
  _msg=(
    "Updating '${_repo}' from '${_bundle}'."
  )
  msg \
    "${_msg[*]}"
  git \
    -C \
      "${_repo}" \
      remote \
        add \
          "${_bundle_name}" \
          "${_bundle}" ||
  true
  git \
    -C \
      "${_repo}" \
    pull \
      "${_bundle_name}" || \
  true
}

prepare() {
  local \
    _msg=()
  if [[ "${_evmfs}" == "true" ]]; then
    if [[ "${_git}" == "false" ]]; then
      ur \
        "${_like}"
    elif [[ "${_git}" == "true" ]]; then
      _git_unbundle \
        "${_tarname}"
      _git_unbundle_update \
        "${srcdir}/${_tarname}" \
        "${srcdir}/${_bundle_tag_file}"
    fi
  fi
  mkdir \
    -p \
    "${_pkg}-build" \
    "lib32-${_pkg}-build"
  if [[ -d "${_tarname}" ]]; then
    ln \
      -s \
      "${_tarname}" \
      "${_pkg}"
  else
    _msg=(
      "Source not found."
    )
    echo \
      "${_msg[*]}"
  fi
  cd \
    "${_pkg}"
}

build() {
  local \
    _configure_flags=() \
    _configure_opts=() \
    _configure_opts_multilib=() \
    _config_params=() \
    _config_params_multilib=() \
    _make_opts_locale=()
  _configure_flags+=(
    --prefix="/usr"
    --with-headers="/usr/include"
    --with-bugurl="https://github.com/${_ns}/${_pkg}/-/issues"
    --enable-bind-now
    --enable-fortify-source
    --enable-kernel=4.4
    --enable-multi-arch
    --enable-stack-protector="strong"
    --enable-systemtap
    --disable-nscd
    --disable-profile
    --disable-werror
  )
  _config_params+=(
    "slibdir=/usr/lib"
    "rtlddir=/usr/lib"
    "sbindir=/usr/bin"
    "rootsbindir=/usr/bin"
  )
  _config_params_multilib+=(
    "slibdir=/usr/lib32"
    "rtlddir=/usr/lib32"
    "sbindir=/usr/bin"
    "rootsbindir=/usr/bin"
  )
  # _FORTIFY_SOURCE=3 causes testsuite build
  # failure and is unnecessary during
  # actual builds (support is
  # built-in via --enable-fortify-source).
  CFLAGS=${CFLAGS/-Wp,-D_FORTIFY_SOURCE=3/}
  # locale-gen segfaults without this on glibc 2.42
  if [[ ${CARCH} = "aarch64" ]]; then
     CFLAGS=${CFLAGS/-fno-plt/}
     _configure_flags+=(
       --enable-memory-tagging
     )
  fi
  _configure_opts+=(
    --libdir="/usr/lib"
    --libexecdir="/usr/lib"
    --enable-cet
    --enable-sframe
    "${_configure_flags[@]}"
  )
  _configure_opts_multilib+=(
    --host="i686-pc-linux-gnu"
    --libdir="/usr/lib32"
    --libexecdir="/usr/lib32"
    "${_configure_flags[@]}"
  )
  _make_opts_locale+=(
    -C
      "${srcdir}/${_pkg}/localedata"
    objdir="${srcdir}/${_pkg}-build"
    DESTDIR="${srcdir}/locales"
  )
  (
    cd \
      "${_pkg}-build"
    printf \
      "%s\n" \
      "${_config_params[@]}" >> \
      "configparms"
    "${srcdir}/${_pkg}/configure" \
      "${_configure_opts[@]}"
    make \
      -O
    if [[ "${_docs}" == "true" ]]; then
      # build info pages manually
      # for reproducibility
      make \
        info
    fi
  )
  if [[ "${_multilib}" == "true" ]]; then
    if [[ "${CARCH}" == "x86_64"* ]]; then (
      cd \
        "lib32-${_pkg}-build"
      export \
        CC="gcc -m32 -mstackrealign" \
        CXX="g++ -m32 -mstackrealign"
      # remove frame pointer flags due to crashes
      # of nvidia driver on steam starts
      # See https://gitlab.archlinux.org/archlinux/packaging/packages/glibc/-/issues/10
      CFLAGS=${CFLAGS/-fno-omit-frame-pointer -mno-omit-leaf-frame-pointer/}
      _cflags=(
        "${CFLAGS}"
	# -lunwind
      )
      printf \
        "%s\n" \
        "${_config_params_multilib[@]}" >> \
        "configparms"
      "${srcdir}/${_pkg}/configure" \
        "${_configure_opts_multilib[@]}"
      make \
        -O
    )
    fi
  fi
  # pregenerate locales here
  # instead of in package
  # functions because localedef
  # does not like fakeroot
  make \
    "${_make_opts_locale[@]}" \
    install-locale-files
}

# Credits for _skip_test() and check() @allanmcrae
# https://github.com/allanmcrae/toolchain/blob/f18604d70c5933c31b51a320978711e4e6791cf1/glibc/PKGBUILD
_skip_test() {
  test=${1}
  file=${2}
  sed \
    -i \
    "/\b${test} /d" \
    "${srcdir}/${_pkg}/${file}"
}

check() (
  cd \
    "${_pkg}-build"
  # adjust/remove buildflags that cause
  # false-positive testsuite failures
  # failure to build testsuite
  sed \
    -i \
    's/-Werror=format-security/-Wformat-security/' \
    "config.make"
  # 27 failures
  sed \
    -i \
    '/CFLAGS/s/-fno-plt//' \
    "config.make"
  # 1 failure
  sed \
    -i \
    '/CFLAGS/s/-fexceptions//' \
    "config.make"
  # The following tests fail due to
  # restrictions in the Arch build system
  # The correct fix is to add the
  # following to the systemd-nspawn call:
  # --system-call-filter="@clock @memlock @pkey"
  _skip_test \
    "test-errno-linux" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-mlock2" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-ntp_gettime" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-ntp_gettimex" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-pkey" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-mseal-pkey" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-process_mrelease" \
    "sysdeps/unix/sysv/linux/Makefile"
  _skip_test \
    "tst-shstk-legacy-1g" \
    "sysdeps/x86_64/Makefile"
  _skip_test \
    "tst-adjtime" \
    "time/Makefile"
  make \
    -O \
    check
)

package_glibc() {
  pkgdesc='GNU C Library'
  depends=(
    'linux-api-headers>=4.10'
    "tzdata"
    "filesystem"
  )
  optdepends=(
    'gd: for memusagestat'
    'perl: for mtrace'
  )
  install="${_pkg}.install"
  backup=(
    "etc/gai.conf"
    "etc/locale.gen"
  )
  make \
    -C \
      "${_pkg}-build" \
    DESTDIR="${pkgdir}" \
    install
  rm \
    -f \
    "${pkgdir}/etc/ld.so.cache"
  # Shipped in tzdata
  rm \
    -f \
    "${pkgdir}/usr/bin/"{"tzselect","zdump","zic"}
  cd \
    "${_pkg}"
  install \
    -vdm755 \
    "${pkgdir}/usr/lib/locale"
  install \
    -vm644 \
    "posix/gai.conf" \
    "${pkgdir}/etc/gai.conf"
  install \
    -vm755 \
    "${srcdir}/locale-gen" \
    "${pkgdir}/usr/bin"
  # Create /etc/locale.gen
  install \
    -vm644 \
    "${srcdir}/locale.gen.txt" \
    "${pkgdir}/etc/locale.gen"
  sed \
    -e \
      '1,3d' \
    -e \
      's|/| |g' \
    -e \
      's|\\| |g' \
    -e \
      's|^|#|g' \
    "localedata/SUPPORTED" >> \
    "${pkgdir}/etc/locale.gen"
  # Add SUPPORTED file to pkg
  sed \
    -e \
      '1,3d' \
    -e \
      's|/| |g' \
    -e \
      's| \\||g' \
    "localedata/SUPPORTED" > \
    "${pkgdir}/usr/share/i18n/SUPPORTED"
  # install C.UTF-8 so that it is always available
  # should be built into glibc eventually
  # https://sourceware.org/glibc/wiki/Proposals/C.UTF-8
  # https://bugs.archlinux.org/task/74864
  install \
    -vdm755 \
    "${pkgdir}/usr/lib/locale"
  cp \
    -r \
    "${srcdir}/locales/usr/lib/locale/C.utf8" \
    -t \
    "${pkgdir}/usr/lib/locale"
  sed \
    -i \
    '/#C\.UTF-8 /d' \
    "${pkgdir}/etc/locale.gen"
  # Provide tracing probes to libstdc++ for exceptions, possibly for other
  # libraries too. Useful for gdb's catch command.
  install \
    -vDm644 \
    "${srcdir}/sdt.h" \
    "${pkgdir}/usr/include/sys/sdt.h"
  install \
    -vDm644 \
    "${srcdir}/sdt-config.h" \
    "${pkgdir}/usr/include/sys/sdt-config.h"
}

package_lib32-glibc() {
  pkgdesc='GNU C Library (32-bit)'
  depends=(
    "${_pkg}=$pkgver"
  )
  options+=(
    '!emptydirs'
  )
  install="lib32-${_pkg}.install"
  arch=(
    x86_64
  )
  cd \
    "lib32-${_pkg}-build"
  make \
    DESTDIR="${pkgdir}" \
    install
  rm \
    -rf \
    "${pkgdir}/"{"etc","sbin","usr/"{"bin","sbin","share"},"var"}
  # We need to keep 32 bit
  # specific header files
  find \
    "${pkgdir}/usr/include" \
    -type \
      "f" \
    -not \
    -name \
      '*-32.h' \
    -delete
  # Dynamic linker
  install \
    -d \
    "${pkgdir}/usr/lib"
  ln \
    -s \
    "../lib32/ld-linux.so.2" \
    "${pkgdir}/usr/lib/"
  # Add lib32 paths to the default library search path
  install \
    -vDm644 \
    "${srcdir}/lib32-${_pkg}.conf" \
    "${pkgdir}/etc/ld.so.conf.d/lib32-${_pkg}.conf"
  # Symlink /usr/lib32/locale to /usr/lib/locale
  ln \
    -s \
    "../lib/locale" \
    "${pkgdir}/usr/lib32/locale"
}

package_glibc-locales() {
  pkgdesc='Pregenerated locales for GNU C Library'
  depends=(
    "glibc=$pkgver"
  )
  cp \
    -r \
    "locales/"* \
    -t \
    "${pkgdir}"
  rm \
    -r \
    "${pkgdir}/usr/lib/locale/C.utf8"
  # deduplicate locale data
  hardlink \
    -c \
    "${pkgdir}/usr/lib/locale"
}
