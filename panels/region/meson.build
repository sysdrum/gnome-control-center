panel_list += cappletname
desktop = 'gnome-' + cappletname + '-panel.desktop'

desktop_in = configure_file(
  input: desktop + '.in.in',
  output: desktop + '.in',
  configuration: desktop_conf
)

i18n.merge_file(
  desktop,
  type: 'desktop',
  input: desktop_in,
  output: desktop,
  po_dir: po_dir,
  install: true,
  install_dir: control_center_desktopdir
)

sources = files(
  'cc-' + cappletname + '-panel.c',
  'cc-format-chooser.c',
  'cc-ibus-utils.c',
  'cc-input-chooser.c',
  'cc-input-options.c'
)

resource_data = files(
  'format-chooser.ui',
  'input-chooser.ui',
  'input-options.ui',
  'region.ui'
)

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  accounts_dep,
  gnome_desktop_dep,
  libgd_dep,
  polkit_gobject_dep
]

if enable_ibus
  deps += dependency('ibus-1.0', version: '>= 1.5.2')
endif

libregion = static_library(
  cappletname,
  sources: sources,
  include_directories: [top_inc, common_inc],
  dependencies: deps,
  link_with: liblanguage
)
