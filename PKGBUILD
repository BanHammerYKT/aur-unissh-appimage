# Maintainer: BanHammer  <no@e.mail>

pkgname="unissh-appimage"
pkgver=0.3.1
pkgrel=1
pkgdesc="A cross-platform SSH client with end-to-end-encrypted vaults that sync through a server you host."
arch=('x86_64')
url="https://unissh.dev/"
license=('MIT' 'Apache-2.0')
depends=('zlib' 'hicolor-icon-theme' 'fuse2')
options=(!strip !debug)
_appimage="${pkgname}-${pkgver}.AppImage"
source_x86_64=("${_appimage}::https://github.com/goduni/unissh/releases/download/v${pkgver}/UniSSH_${pkgver}_amd64.AppImage")
noextract=("${_appimage}")
sha256sums_x86_64=('SKIP')
appname="UniSSH"
_appname="unissh"

prepare() {
    chmod +x "${_appimage}"
    ./"${_appimage}" --appimage-extract
}

build() {
    # Adjust .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=${_appname}|Exec=env DESKTOPINTEGRATION=false /usr/bin/${_appname}|"\
        "squashfs-root/${appname}.desktop"
    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {
    # AppImage
    install -Dm755 "${srcdir}/${_appimage}" "${pkgdir}/opt/${pkgname}/${pkgname}.AppImage"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/${appname}.desktop"\
            "${pkgdir}/usr/share/applications/${appname}.desktop"

    # Icon images
    install -dm755 "${pkgdir}/usr/share/"
    cp -a "${srcdir}/squashfs-root/usr/share/icons" "${pkgdir}/usr/share/"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}.AppImage" "${pkgdir}/usr/bin/${_appname}"
}
