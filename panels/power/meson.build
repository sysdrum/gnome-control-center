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

resource_data = files('power.ui')

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  gnome_settings_dep,
  upower_glib_dep
]

if have_bluetooth
  deps += gnome_bluetooth_dep
endif

if have_network_manager
  deps += network_manager_deps
endif

cflags = '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)

libpower = static_library(
  cappletname,
  sources: sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)
