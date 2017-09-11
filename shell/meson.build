subdir('appdata')
subdir('alt')
subdir('completions')

service_conf = configuration_data()
service_conf.set('bindir', control_center_bindir)

service = 'org.gnome.ControlCenter.service'

configure_file(
  input: service + '.in',
  output: service,
  install: true,
  install_dir: join_paths(control_center_datadir, 'dbus-1', 'services'),
  configuration: service_conf
)

desktop = meson.project_name() + '.desktop'

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

cflags = ['-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)]

libshell = static_library(
  'shell',
  sources: 'cc-shell-model.c',
  include_directories: [top_inc, common_inc],
  dependencies: common_deps,
  c_args: cflags
)

common_sources = files(
  'cc-application.c',
  'cc-editable-entry.c',
  'cc-hostname-entry.c',
  'cc-panel-loader.c',
  'cc-panel.c',
  'cc-shell-category-view.c',
  'cc-shell-item-view.c',
  'cc-shell-log.c',
  'cc-shell.c',
  'hostname-helper.c',
  'list-box-helper.c',
  'main.c'
)

resource_data = files(
  'help-overlay.ui',
  'panel-list.ui',
  'window.ui'
)

common_sources += gnome.compile_resources(
  'resources',
  meson.project_name() + '.gresource.xml',
  source_dir: '.',
  dependencies: resource_data,
  export: true
)

sources = common_sources + files(
  'cc-panel-list.c',
  'cc-window.c'
)

shell_deps = common_deps + [
  libgd_dep,
  polkit_gobject_dep,
  x11_dep
]

if have_cheese
  shell_deps += cheese_deps
endif

libs = [
  liblanguage,
  libdevice,
  libbackground,
  libpower,
  libcolor,
  libdisplay,
  libmouse_properties,
  libnotifications,
  libonline_accounts,
  libregion,
  libinfo,
  libsound,
  libkeyboard,
  libuniversal_access,
  libuser_accounts,
  libdatetime,
  libsearch,
  libprivacy,
  libsharing,
  libprinters,
  libshell
]

if have_wacom
  libs += libwacom_properties
endif

if have_network_manager
  libs += libnetwork
endif

if have_bluetooth
  libs += libbluetooth
endif

executable(
  meson.project_name(),
  sources,
  include_directories: [top_inc, common_inc],
  dependencies: shell_deps,
  c_args: cflags,
  link_with: libs,
  install: true,
  install_dir: control_center_bindir
)

executable(
  meson.project_name() + '-alt',
  common_sources,
  include_directories: top_inc,
  dependencies: shell_deps,
  c_args: cflags + ['-DCC_ENABLE_ALT_CATEGORIES'],
  link_with: libs + [libshell_alt]
)

# Because it is confusing and somewhat problematic to directly add and compile
# cc-panel-loader.o by another directory (i.e. the shell search provider), we
# have to create a library and link it there, just like libshell.la.
libpanel_loader = static_library(
  'panel_loader',
  sources: 'cc-panel-loader.c',
  include_directories: top_inc,
  dependencies: common_deps,
  c_args: cflags + ['-DCC_PANEL_LOADER_NO_GTYPES']
)

test_unit = 'test-hostname'

sources = files(
  test_unit + '.c',
  'hostname-helper.c'
)

cflags = cflags + [
  '-DTEST_SRCDIR="@0@"'.format(meson.current_source_dir()),
  '-DTEST_TOPSRCDIR="@0@"'.format(meson.source_root())
]

exe = executable(
  test_unit,
  sources,
  include_directories: top_inc,
  dependencies: common_deps,
  c_args: cflags
)

test(test_unit, exe)
