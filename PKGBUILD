# Maintainer: DuckSoft <realducksoft at gmail dot com>
# Co-Maintainer: Mark Wagie <mark dot wagie at tutanota dot com>
# Contributor: KokaKiwi <kokakiwi+aur@kokakiwi.net>

pkgname=ventoy-bin
pkgver=1.0.18
pkgrel=1
pkgdesc='A new multiboot USB solution (Binary)'
url='http://www.ventoy.net/'
arch=('i686' 'x86_64')
license=('GPL3')
depends=('bash' 'util-linux' 'xz' 'exfat-utils')
provides=("${pkgname%-bin}")
conflicts=("${pkgname%-bin}")
source=("https://github.com/ventoy/Ventoy/releases/download/v${pkgver}/${pkgname%-bin}-${pkgver}-linux.tar.gz"
        "${pkgname%-bin}" 'sanitize.patch')
sha256sums=('76aa7acd5ce6a49f7a77275d5ce7f8c500181ef89d17daade8778cff1f194e20'
            'f9f333e21ec211318ab40018e0da3a44d8bae16e36dd6132e7891f8c9df52f1c'
            'b042d5b525427a65b566aaf6bef5fd3091c4f543fcd8e3e23055fd35b7d73dbb')

prepare() {
  msg2 "Decompress tools..."
  cd "$srcdir/${pkgname%-bin}-${pkgver}/tool"
  for file in $(ls *.xz); do
    xzcat $file > ${file%.xz}
    chmod +x ${file%.xz}
  done

  msg2 "Cleaning up .xz crap..."
  rm -fv ./*.xz

  msg2 "Applying sanitize patch..."
  cd ..
  patch -p0 < "$srcdir/sanitize.patch"

  msg2 "Cleaning up unused binaries..."
  rm -fv tool/{mkextfatfs_{32,64},mount.exfat-fuse_{32,64},ash,hexdump,xzcat}
}

package() {
  cd "$srcdir/${pkgname%-bin}-${pkgver}"

  msg2 "Copying package files..."
  install -Dm644 -t "$pkgdir/opt/${pkgname%-bin}/boot/"            boot/*
  install -Dm644 -t "$pkgdir/opt/${pkgname%-bin}/${pkgname%-bin}/" "${pkgname%-bin}"/*
  install -Dm755 -t "$pkgdir/opt/${pkgname%-bin}/tool/"            tool/*
  install -Dm755 -t "$pkgdir/opt/${pkgname%-bin}/"                 *.sh

  msg2 "Linking system binaries..."
  ln -svf /usr/bin/xzcat   "$pkgdir/opt/${pkgname%-bin}/tool/"
  ln -svf /usr/bin/hexdump "$pkgdir/opt/${pkgname%-bin}/tool/"
  ln -svf /usr/bin/mkfs.exfat "$pkgdir/opt/${pkgname%-bin}/tool/mkextfatfs_32"
  ln -svf /usr/bin/mkfs.exfat "$pkgdir/opt/${pkgname%-bin}/tool/mkextfatfs_64"
  ln -svf /usr/bin/mount.exfat-fuse "$pkgdir/opt/${pkgname%-bin}/tool/mount.exfat-fuse_32"
  ln -svf /usr/bin/mount.exfat-fuse "$pkgdir/opt/${pkgname%-bin}/tool/mount.exfat-fuse_64"

  msg2 "Creating /usr/bin entries..."
  install -Dm755 "$srcdir/${pkgname%-bin}" -t "$pkgdir"/usr/bin/
}
