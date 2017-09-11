project(
  'gnome-control-center', 'c',
  version: '3.26.0',
  license: 'GPL2',
  default_options: [
    'buildtype=debugoptimized',
    'warning_level=1'
  ],
  meson_version: '>= 0.43.0'
)

control_center_version = meson.project_version()

control_center_api_version = '2.0'
control_center_api_name = '@0@-@1@'.format(meson.project_name(), control_center_api_version)

control_center_prefix = get_option('prefix')
control_center_bindir = join_paths(control_center_prefix, get_option('bindir'))
control_center_datadir = join_paths(control_center_prefix, get_option('datadir'))
control_center_libexecdir = join_paths(control_center_prefix, get_option('libexecdir'))
control_center_localedir = join_paths(control_center_prefix, get_option('localedir'))
control_center_mandir = join_paths(control_center_prefix, get_option('mandir'))
control_center_sysconfdir = join_paths(control_center_prefix, get_option('sysconfdir'))

control_center_pkgdatadir = join_paths(control_center_datadir, meson.project_name())

control_center_desktopdir = join_paths(control_center_datadir, 'applications')

control_center_buildtype = get_option('buildtype')
control_center_debug = control_center_buildtype.contains('debug')

host_is_linux = host_machine.system().contains('linux')
host_is_s390 = host_machine.cpu().contains('s390')

cc = meson.get_compiler('c')

config_h = configuration_data()

# defines
set_defines = [
  # package
  ['PACKAGE', meson.project_name()],
  ['PACKAGE_BUGREPORT', 'http://bugzilla.gnome.org/enter_bug.cgi?product=' + meson.project_name()],
  ['PACKAGE_NAME', meson.project_name()],
  ['PACKAGE_STRING', '@0@ @1@'.format(meson.project_name(), control_center_version)],
  ['PACKAGE_TARNAME', meson.project_name()],
  ['PACKAGE_URL', ''],
  ['PACKAGE_VERSION', control_center_version],
  ['VERSION', control_center_version],
  # i18n
  ['GETTEXT_PACKAGE', control_center_api_name]
]

foreach define: set_defines
  config_h.set_quoted(define[0], define[1])
endforeach

# debug
config_h.set('NDEBUG', not control_center_debug)
config_h.set('GNOME_ENABLE_DEBUG', control_center_debug)

config_h.set('USER_DIR_MODE', 0700,
             description: 'Permissions for creating the user\'s config, cache and data directories')

# headers
check_headers = [
  ['HAVE_DLFCN_H', 'dlfcn.h'],
  ['HAVE_INTTYPES_H', 'inttypes.h'],
  ['HAVE_MEMORY_H', 'memory.h'],
  ['HAVE_STDINT_H', 'stdint.h'],
  ['HAVE_STDLIB_H', 'stdlib.h'],
  ['HAVE_STRINGS_H', 'strings.h'],
  ['HAVE_STRING_H', 'string.h'],
  ['HAVE_SYS_STAT_H', 'sys/stat.h'],
  ['HAVE_SYS_TYPES_H', 'sys/types.h'],
  ['HAVE_UNISTD_H', 'unistd.h']
]

foreach header: check_headers
  config_h.set(header[0], cc.has_header(header[1]))
endforeach

# functions
check_functions = [
  # i18n
  ['HAVE_DCGETTEXT', 'dcgettext'],
  ['HAVE_GETTEXT', 'gettext']
]

foreach func: check_functions
  config_h.set(func[0], cc.has_function(func[1]))
endforeach

# compiler flags
common_flags = ['-DHAVE_CONFIG_H']

# Only add this when optimizing is enabled (default)
optimized_src = '''
  #if __OPTIMIZE__ == 0
  #error No optimization
  #endif
'''

control_center_optimized = control_center_buildtype.contains('optimized') and cc.compiles(optimized_src)
message('whether_optimization is enabled: ' + control_center_optimized.to_string())

if control_center_optimized
  common_flags += '-Wp,-D_FORTIFY_SOURCE=2'
endif

if control_center_debug
  test_flags = [
    '-Wcast-align',
    '-Wmissing-field-initializers',
    '-Wmissing-declarations',
    '-Wmissing-prototypes',
    '-Wnested-externs',
    '-Wno-strict-aliasing',
    '-Wno-sign-compare'
  ]

  foreach flag: test_flags
    if cc.has_argument(flag)
      common_flags += [flag]
    endif
  endforeach
endif

add_project_arguments(common_flags, language: 'c')

# Check that we meet the  dependencies
libgvc = subproject(
  'gvc',
  default_options: 'static=true'
)
libgvc_dep = libgvc.get_variable('libgvc_dep')

libgd = subproject(
  'libgd',
  default_options: [
    'static=true',
    'with-view-common=true',
    'with-main-view=true'
  ]
)
libgd_dep = libgd.get_variable('libgd_dep')

goa_req_version = '>= 3.25.3'
pulse_req_version = '>= 2.0'

accounts_dep = dependency('accountsservice', version: '>= 0.6.39')
clutter_gtk_dep = dependency('clutter-gtk-1.0', required: false)
colord_dep = dependency('colord', version: '>= 0.1.34')
gdk_pixbuf_dep = dependency('gdk-pixbuf-2.0', version: '>= 2.23.0')
gio_dep = dependency('gio-2.0')
glib_dep = dependency('glib-2.0', version: '>= 2.53.0')
gnome_desktop_dep = dependency('gnome-desktop-3.0', version: '>= 3.1.0')
gnome_settings_dep = dependency('gnome-settings-daemon', version: '>= 3.25.90')
goa_dep = dependency('goa-1.0', version: goa_req_version)
gsettings_desktop_dep = dependency('gsettings-desktop-schemas', version: '>= 3.21.4')
libxml_dep = dependency('libxml-2.0')
polkit_gobject_dep = dependency('polkit-gobject-1', version: '>= 0.103')
pulse_dep = dependency('libpulse', version: pulse_req_version)
pulse_mainloop_dep = dependency('libpulse-mainloop-glib', version: pulse_req_version)
upower_glib_dep = dependency('upower-glib', version: '>= 0.99.0')
x11_dep = dependency('x11')
xi_dep = dependency('xi', version: '>= 1.2')

m_dep = cc.find_library('m')

common_deps = [
  gio_dep,
  glib_dep,
  gsettings_desktop_dep,
  dependency('gio-unix-2.0'),
  dependency('gthread-2.0'),
  dependency('gtk+-3.0', version: '>= 3.22.0')
]

# x11 headers
xf86misc_dep = dependency('Xxf86misc', required: false)
if xf86misc_dep.found() and cc.has_function('XF86MiscQueryExtension', dependencies: xf86misc_dep)
  has_header = cc.has_header('X11/extensions/xf86misc.h', dependencies: xf86misc_dep)
  config_h.set('HAVE_X11_EXTENSIONS_XF86MISC_H', has_header)
endif

check_headers = [
  ['HAVE_X11_EXTENSIONS_XKB_H', 'X11/extensions/XKB.h'],
  ['HAVE_X11_XLIB_H', 'X11/Xlib.h']
]

foreach header: check_headers
  config_h.set(header[0], cc.has_header(header[1], dependencies: x11_dep))
endforeach

# udev
udev_dep = dependency('gudev-1.0', required: false)
have_udep = udev_dep.found()

config_h.set('HAVE_UDEV', have_udep,
             description: ' System has udev')

# network manager
libnm_dep = dependency('libnm', version: '>= 1.2.0', required: false)
libnma_dep = dependency('libnma', version: '>= 1.2.0', required: false)
mm_glib_dep = dependency('mm-glib', version: '>= 0.7', required: false)
have_network_manager = libnm_dep.found() and libnma_dep.found() and mm_glib_dep.found()

if have_network_manager
  network_manager_dep = dependency('NetworkManager')
  nm_vpn_config_dir = join_paths(network_manager_dep.get_pkgconfig_variable('configdir'), 'VPN')
  nm_vpn_module_dir = network_manager_dep.get_pkgconfig_variable('plugindir')
else
  error_str = '*** Network panel will not be built (NetworkManager or ModemManager not found) ***'
  if host_is_linux
    error(error_str)
  endif
  message(error_str)
endif

config_h.set('BUILD_NETWORK', have_network_manager,
             description: 'Define to 1 to build the Network panel')
config_h.set('HAVE_NETWORK_MANAGER', have_network_manager,
             description: 'Define to 1 if NetworkManager is available')

network_manager_deps = [
  libnm_dep,
  libnma_dep,
  mm_glib_dep
]

# Check for gnome-bluetooth
gnome_bluetooth_dep = dependency('gnome-bluetooth-1.0', version: '>= 3.18.2', required: false)
have_bluetooth = gnome_bluetooth_dep.found()

if not have_bluetooth
  error_str = '*** Bluetooth panel will not be built'
  if host_is_linux
    if not host_is_s390
      error(error_str + ' ***')
    endif
    message(error_str + ' (no USB support on this platform) ***')
  else
    message(error_str + ' ***')
  endif
endif

config_h.set('BUILD_BLUETOOTH', have_bluetooth,
             description: 'Define to 1 to build the Bluetooth panel')
config_h.set('HAVE_BLUETOOTH', have_bluetooth,
             description: 'Define to 1 if bluetooth support is available')

# Check for CUPS 1.4 or newer
cups_dep = dependency('cups', version : '>= 1.4', required: false)
assert(cups_dep.found(), 'CUPS 1.4 or newer not found')

# https://bugzilla.gnome.org/show_bug.cgi?id=696766
cups_cflags = []
# FIXME: This produces a lot of errors
'''
if cups_dep.version().version_compare('>= 1.6')
  cups_cflags += '-D_PPD_DEPRECATED=""'
endif
'''

# cups headers
check_headers = [
  ['HAVE_CUPS_CUPS_H', 'cups/cups.h'],
  ['HAVE_CUPS_HTTP_H', 'cups/http.h'],
  ['HAVE_CUPS_IPP_H', 'cups/ipp.h'],
  ['HAVE_CUPS_PPD_H', 'cups/ppd.h']
]

foreach header: check_headers
  has_header = cc.has_header(header[1], args: cups_cflags)
  assert(has_header, 'CUPS headers not found: ' + header[1])
  config_h.set(header[0], has_header)
endforeach

# Optional dependency for the user accounts panel
have_cheese = false
enable_cheese = get_option('enable-cheese')
if enable_cheese != 'no'
  cheese_gtk_dep = dependency('cheese-gtk', version: '>= 3.5.91', required: false)
  cheese_dep = dependency('cheese', required: false)
  have_cheese = cheese_gtk_dep.found() and cheese_dep.found() and clutter_gtk_dep.found()

  if have_cheese
    cheese_deps = [
      cheese_gtk_dep,
      cheese_dep,
      clutter_gtk_dep
    ]
  else
    error_str = 'Cheese configured but not found'
    if enable_cheese == 'yes'
      error(error_str)
    endif
    message(error_str)
  endif
endif

config_h.set('HAVE_CHEESE', have_cheese,
             description: 'Define to 1 to enable cheese webcam support')

# Wacom
clutter_dep = dependency('clutter-1.0', version: '>= 1.11.3', required: false)
libwacom_dep = dependency('libwacom', version: '>= 0.7', required: false)
have_wacom = clutter_dep.found() and clutter_gtk_dep.found() and libwacom_dep.found()

if not have_wacom
  error_str = '*** Wacom panel will not be built'
  if host_is_linux
    if not host_is_s390
      error(error_str + ' ***')
    endif
    message(error_str + ' (no USB support on this platform) ***')
  else
    message(error_str + ' ***')
  endif
endif

config_h.set('BUILD_WACOM', have_wacom,
             description: 'Define to 1 to build the Wacom panel')
config_h.set('HAVE_WACOM', have_wacom,
             description: 'Define to 1 is Wacom is supportted')

# Check for info panel
gnome_session_libexecdir = get_option('with-gnome-session-libexecdir').strip()
if gnome_session_libexecdir == ''
  gnome_session_libexecdir = control_center_libexecdir
endif

# IBus support
enable_ibus = get_option('enable-ibus')
config_h.set('HAVE_IBUS', enable_ibus,
             description: 'Defined if IBus support is enabled')

configure_file(
  output: 'config.h',
  configuration: config_h
)

gnome = import('gnome')
i18n = import('i18n')
pkg = import('pkgconfig')

desktop_conf = configuration_data()
desktop_conf.set('VERSION', control_center_version)

po_dir = join_paths(meson.source_root(), 'po')
its_dir = join_paths(meson.source_root(), 'gettext')

top_inc = include_directories('.')
shell_inc = include_directories('shell')

update_from_gsd = find_program('update-from-gsd.sh')
update_from_gsd_in = files('update-from-gsd.in')
update_from_nma_in = files('update-from-nma.in')

subdir('po')
subdir('panels')
subdir('shell')
subdir('search-provider')

if get_option('enable-documentation')
  subdir('man')
endif

meson.add_install_script(
  'meson_post_install.py',
  control_center_datadir
)

output = meson.project_name() + ' was configured with the following options:\n'

if have_network_manager
  output += '** NetworkManager (Network panel)\n'
else
  output += '   Network panel disabled\n'
endif

if have_bluetooth
  output += '** gnome-bluetooth (Bluetooth panel)\n'
else
  output += '   Bluetooth panel disabled\n'
endif

if have_cheese
  output += '** Cheese (Users panel webcam support)\n'
else
  output += '   Users panel webcam support disabled\n'
endif

if have_wacom
  output += '** wacom (Wacom tablet panel)\n'
else
  output += '   Wacom panel disabled\n'
endif

if enable_ibus
  output += '** IBus (Region panel IBus support)\n'
else
  output += '  Region panel IBus support disabled\n'
endif

output += '** gnome-session libexecdir: ' + gnome_session_libexecdir + '\n'
output += 'End options'
message(output)
