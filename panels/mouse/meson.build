panel_list += cappletname
desktop = 'gnome-' + cappletname + '-panel.desktop'

desktop_in = configure_file(
  input: desktop + '.in.in',
  output: desktop + '.in',
  configuration: desktop_conf
)

custom_target(
  desktop,
  input: desktop_in,
  output: desktop,
  command: intltool_desktop_cmd,
  install: true,
  install_dir: control_center_desktopdir
)

resource_data = files(
  'gnome-mouse-properties.ui',
  'gnome-mouse-test.ui',
  'scroll-test-gegl.svg',
  'scroll-test.svg'
)

common_sources = gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

sources = common_sources + files(
  'cc-' + cappletname + '-panel.c',
  'cc-mouse-caps-helper.c',
  'gnome-mouse-properties.c',
  'gnome-mouse-test.c'
)

deps = common_deps + [
  gnome_settings_dep,
  x11_dep,
  xi_dep
]

cflags = '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)

libmouse_properties = static_library(
  cappletname + '-properties',
  sources: sources,
  include_directories: [top_inc, common_inc],
  dependencies: deps,
  c_args: cflags,
  link_with: libdevice
)

name = 'gnome-mouse-test'

sources = common_sources + files(
  name + '.c',
  'test-' + name + '.c'
)

executable(
  'test-' + name,
  sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)
