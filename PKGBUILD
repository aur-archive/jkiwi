# Maintainer: Alexander Rødseth <rodseth@gmail.com>
# Contributor: Dany Martineau <dany.luc.martineau@gmail.com>

pkgname=jkiwi
pkgver=0.9.5
pkgrel=5
pkgdesc="Virtual makeover and hairstyler application"
arch=('x86_64' 'i686')
url="http://www.jkiwi.com/" 
license=('GPL3')
depends=('java-runtime' 'yelp')
makedepends=('jdk6' 'imagemagick')
if [ "$CARCH" = "x86_64" ]; then
  source=("http://downloads.sourceforge.net/$pkgname/jKiwi-0.9.5_linux-x86_64.tar.bz2" "jkiwi.desktop")
  md5sums=('643e6c3f2b1e9d772ab94350c4c4074b' '2f4654eb722fb0ea3783e70e49ab047a')
else
  source=("http://downloads.sourceforge.net/$pkgname/jKiwi-0.9.5_linux.tar.bz2" "jkiwi.desktop")
  md5sums=('8c47660c85a18068e614e0f26521cdbf' '2f4654eb722fb0ea3783e70e49ab047a')
fi

build() { 
  cd "$srcdir/jKiwi-$pkgver/src"

  msg2 "Fixing source..."
  sed -i 's/long style = (Integer) gtk_widget/long style = (Long) gtk_widget/' \
    utils/GtkStockIconSWT.java
  sed -i 's/long pixbuf = (Integer) gtk_icon/long pixbuf = (Long) gtk_icon/' \
    utils/GtkStockIconSWT.java
  sed -i 's/long pixels = (Integer) gdk_pixbuf/long pixels = (Long) gdk_pixbuf/' \
    utils/GtkStockIconSWT.java
  msg2 "Compiling..."
  javac -classpath \
    ../lib/in_use/metadata-extractor.jar:../lib/in_use/swt.jar @classes
  jar -cfm ../bin/jkiwi.jar MANIFEST.MF *
  cd ..
  msg2 "Creating wrapperscript..."
  echo -e '#!/bin/sh\ncd /usr/share/jkiwi\n./jKiwi' > "$pkgname.sh"
  msg2 "Converting icon to .png..."
  convert resources/pixmaps/program_icon.ico "$pkgname.png"
}

package() {
  cd "$srcdir/jKiwi-$pkgver"

  msg2 "Packaging data files..."
  mkdir -p "$pkgdir/usr/share/"{jkiwi,applications} "$pkgdir/usr/bin"
  cp -R bin help lib locale resources AUTHORS NEWS README \
    "$pkgdir/usr/share/$pkgname"
  install -Dm755 jKiwi "$pkgdir/usr/share/$pkgname"
  msg2 "Packaging wrapperscript..."
  install -Dm755 $pkgname.sh "$pkgdir/usr/bin/$pkgname"
  msg2 "Packaging application shortcut..."
  install -Dm644 "$srcdir/$pkgname.desktop" "$pkgdir/usr/share/applications"
  msg2 "Packaging application icon..."
  install -Dm644 "$pkgname-3.png" "$pkgdir/usr/share/pixmaps/$pkgname.png"
  msg2 "Packaging license..."
  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

# vim:set ts=2 sw=2 et:
