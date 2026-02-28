# Maintainer: Xilin Wu <sophon@radxa.com>
# Upstream: https://github.com/qualcomm/gst-plugins-imsdk

pkgname=gst-plugins-imsdk
pkgver=1.0
pkgrel=4
pkgdesc="Qualcomm GStreamer IMSDK plugins for camera, ML inference, video processing and streaming"
arch=('aarch64')
url="https://github.com/qualcomm/gst-plugins-imsdk"
license=('BSD-3-Clause-Clear')
depends=(
  'gstreamer-qti-oss'
  # 'gst-plugins-base'
  # 'gst-plugins-bad'
  # 'gst-plugins-good'
  # 'gst-rtsp-server'
  'qcom-camera-service'
  'librdkafka'
  'qcom-smart-venc-ctrl-algo'
  'mosquitto'
  'qcom-qnn-sdk'
  'qcom-snpe-sdk'
  'hiredis'
  'opencv-fastcv'
  'json-glib'
  'libtensorflow-lite'
  'eigen'
)
makedepends=('git' 'cmake')
source=("${pkgname}::git+https://github.com/qualcomm/gst-plugins-imsdk.git#commit=6a084f46f7e0215ca14927586d8b5166db8d0c00"
        '0001-fix-rtspbin-client-filter-list-leak.patch')
sha256sums=('SKIP' 'SKIP')

prepare() {
  cd "$pkgname"
  git reset --hard HEAD
  git clean -fd
  patch -Np1 -i "$srcdir/0001-fix-rtspbin-client-filter-list-leak.patch"
}

build() {
  # mlonnx seems broken somehow, disable it for now
  cmake -B build -S "$pkgname" \
    -Wno-dev \
    -DENABLE_GST_CAMERA_PLUGINS=ON \
    -DENABLE_GST_IMSDK_PLUGINS=ON \
    -DENABLE_GST_PLUGIN_MLTFLITE=ON \
    -DENABLE_GST_PLUGIN_MLONNX=OFF \
    -DENABLE_GST_PLUGIN_EXAMPLES=ON \
    -DENABLE_GST_PYTHON_EXAMPLES=ON \
    -DENABLE_GST_SAMPLE_APPS=ON \
    -DENABLE_GST_PLUGIN_TOOLS=ON \
    -DENABLE_GST_TEST_FRAMEWORK=OFF \
    -DENABLE_GST_UMD_DAEMON=OFF \
    -DSYSROOT_INCDIR=/usr/include \
    -DSYSROOT_LIBDIR=/usr/lib \
    -DTARGET_BOARD_PLATFORM=kodiak \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_BINDIR=bin \
    -DCMAKE_INSTALL_LIBDIR=lib \
    -DCMAKE_INSTALL_INCLUDEDIR=include \
    -DCMAKE_INSTALL_SYSCONFDIR=/etc \
    -DVHDR_MODES_ENABLE=TRUE
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 "$pkgname/LICENSE.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"
}
