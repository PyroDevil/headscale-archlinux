pkgname=headscale
pkgver=0.17.0alpha4
_pkgver="0.17.0-alpha4"
pkgrel=1
pkgdesc="An open source, self-hosted implementation of the Tailscale coordination server."
arch=('any')
url="https://github.com/juanfont/headscale"
license=('BSD')
depends=()
makedepends=('go')
optdepends=(
	'wireguard-tools: CLI tools for generating keys'
	'postgresql: alternative database provider'
)
conflicts=("${pkgname}-git")
backup=("etc/${pkgname}/config.yaml" "etc/${pkgname}/derp.yaml")

source=(
	"${pkgname}-${_pkgver}.tar.gz::https://github.com/juanfont/headscale/archive/refs/tags/v${_pkgver}.tar.gz"
	'headscale.service'
	'headscale.sysusers'
	'headscale.tmpfiles'
)
sha256sums=('9ef12ffd4884acf3708bb9947cb5e896128c2a8b113cf35f8f9d78734dbd0975'
            'SKIP'
            'SKIP'
            'SKIP')

build() {
	cd "${srcdir}/${pkgname}-${_pkgver}"
	version="v$_pkgver" make
	sed -i 's-/var/run/headscale\.sock-/var/run/headscale/headscale\.sock-' config-example.yaml
}

package() {
	cd "$srcdir/${pkgname}-${_pkgver}"
	install -D -m755 "${pkgname}" "${pkgdir}/usr/bin/${pkgname}"

	install -D -m644 "config-example.yaml" "${pkgdir}/etc/${pkgname}/config.yaml"
	install -D -m644 "config-example.yaml" "${pkgdir}/usr/share/${pkgname}/config-example.yaml"

	install -D -m644 "derp-example.yaml" "${pkgdir}/etc/${pkgname}/derp.yaml"
	install -D -m644 "derp-example.yaml" "${pkgdir}/usr/share/${pkgname}/derp-example.yaml"

	install -D -m644 "${srcdir}/${pkgname}.sysusers" "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
	install -D -m644 "${srcdir}/${pkgname}.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"

	install -D -m644 "LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

	install -D -m644 "${srcdir}/${pkgname}.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}.service"
}
