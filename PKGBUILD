# Maintainer: Eric Cheng <eric@chengeric.com>
# Contributor: Heddxh <g311571057@gmail.com>

pkgbase=jellyfin-bin
pkgname=(jellyfin-bin jellyfin-web-bin jellyfin-server-bin)
pkgver=10.8.13
_pkgver="${pkgver}-1"
pkgrel=1
pkgdesc='The Free Software Media System'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://jellyfin.org/'
license=('GPL2')
provides=('jellyfin')
conflicts=('jellyfin')
source=(
    "jellyfin-web-${pkgver}.deb::https://repo.jellyfin.org/releases/server/debian/versions/stable/web/${pkgver}/jellyfin-web_${_pkgver}_all.deb"
    'jellyfin.conf'
    'jellyfin.service'
    'jellyfin.sysusers'
    'jellyfin.tmpfiles'
)
source_x86_64=("jellyfin-${pkgver}.deb::https://repo.jellyfin.org/releases/server/debian/versions/stable/server/${pkgver}/jellyfin-server_${_pkgver}_amd64.deb")
source_aarch64=("jellyfin-${pkgver}.deb::https://repo.jellyfin.org/releases/server/debian/versions/stable/server/${pkgver}/jellyfin-server_${_pkgver}_arm64.deb")
source_armv7h=("jellyfin-${pkgver}.deb::https://repo.jellyfin.org/releases/server/debian/versions/stable/server/${pkgver}/jellyfin-server_${_pkgver}_armhf.deb")
sha256sums=(
    'c12c56e98b22efe94dfa8a4a66333278db83684947da37bfb6be52f03211698c'
    '1ea19635cced6672484937903c27976a5a145d708caff06a687a8defdd23d549'
    '0f8511673816daf528625366b6c27bc7e6182e4ac789191c87474667398376e2'
    '9bc1ddb77c73d46cc4078356b5773e5a776ebf8b47a1c820ad5fb17591ad5228'
    'b7faa4b0c756cdb361ef5b04fddfdc416b00f1246bb3a19a34bf4d185a6a7e5a'
)
sha256sums_x86_64=('93ada8f1d803d826dc93672dffd11780e4e3d36625ded9ae96b874e91666e134')
sha256sums_aarch64=('26c71339b6af55aaf53a85a6a84a46f14b9eb9a7e3453cf19c6baf8220cba9be')
sha256sums_armv7h=('6fa7e500f4695baff81bdc0af28b14990e995c4d7dd4294a885eb1168e772b0a')
noextract=("jellyfin-${pkgver}.deb" "jellyfin-web-${pkgver}.deb")
options=('staticlibs')

prepare() {
    mkdir -p "jellyfin-web" "jellyfin-server"
    bsdtar -xf "jellyfin-web-${pkgver}.deb" -C "jellyfin-web"
    bsdtar -xf "jellyfin-${pkgver}.deb" -C "jellyfin-server"
}

package_jellyfin-bin() {
    depends=("jellyfin-web-bin=${pkgver}" "jellyfin-server-bin=${pkgver}")
}

package_jellyfin-server-bin() {
    pkgdesc="Jellyfin server component"
    optdepends=('jellyfin-ffmpeg5: Patched FFmpeg providing hardware acceleration and tonemapping support')
    depends=('ffmpeg')
    provides=('jellyfin-server')
    conflicts=('jellyfin-server')
    backup=('etc/conf.d/jellyfin')

    tar -xf "jellyfin-server/data.tar.xz" -C "jellyfin-server"
    cp -r "$srcdir/jellyfin-server/usr" "$pkgdir/usr"
    rm -r "$pkgdir/usr/share"

    install -Dm 644 "jellyfin.service" -t "$pkgdir/usr/lib/systemd/system/"
    install -Dm 644 "jellyfin.sysusers" "$pkgdir/usr/lib/sysusers.d/jellyfin.conf"
    install -Dm 644 "jellyfin.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/jellyfin.conf"
    install -Dm 644 "jellyfin.conf" "$pkgdir/etc/conf.d/jellyfin"
}

package_jellyfin-web-bin() {
    pkgdesc="Jellyfin web client"
    provides=('jellyfin-web')
    conflicts=('jellyfin-web')

    tar -xf "jellyfin-web/data.tar.xz" -C "jellyfin-web"
    cp -r "$srcdir/jellyfin-web/usr" "$pkgdir/"
}
