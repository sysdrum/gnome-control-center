subdir('icons')

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

sources = files('cc-' + cappletname + '-panel.c')

resource_data = files('online-accounts.ui')

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname.underscorify(),
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  goa_dep,
  libgd_dep,
  dependency('goa-backend-1.0', version: goa_req_version)
]

cflags = '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)

libonline_accounts = static_library(
  cappletname,
  sources: sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)
