project(
  'wlr-protocols',
  'c',
  license: 'MIT',
  meson_version: '>=0.48.0',
)

wayland_scanner = find_program('wayland-scanner')
wayland_client = dependency('wayland-client')

wayland_scanner_code = generator(
  wayland_scanner,
  output: '@BASENAME@-protocol.c',
  arguments: ['private-code', '@INPUT@', '@OUTPUT@'],
)

wayland_scanner_client = generator(
  wayland_scanner,
  output: '@BASENAME@-client-protocol.h',
  arguments: ['client-header', '@INPUT@', '@OUTPUT@'],
)

wlr_protocols_dir = 'unstable'

client_protocols = [
  [wlr_protocols_dir, 'wlr-data-control-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-export-dmabuf-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-foreign-toplevel-management-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-gamma-control-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-input-inhibitor-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-layer-shell-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-output-management-unstable-v1.xml'],
  [wlr_protocols_dir, 'wlr-screencopy-unstable-v1.xml'],
]

foreach p : client_protocols
  xml = join_paths(p)
  src = wayland_scanner_code.process(xml)
  header = wayland_scanner_client.process(xml)

  name = p[1].split('.')[0].underscorify()

  lib = static_library(
    name,
    [src, header],
    dependencies: [wayland_client],
  )

  dep = declare_dependency(
    link_with: lib,
    sources: header,
  )

  set_variable(name, dep)
endforeach