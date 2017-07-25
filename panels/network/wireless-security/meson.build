name = 'wireless-security'

wireless_security_inc = include_directories('.')

sources = files(
  'eap-method-fast.c',
  'eap-method-leap.c',
  'eap-method-peap.c',
  'eap-method-simple.c',
  'eap-method-tls.c',
  'eap-method-ttls.c',
  'eap-method.c',
  'helpers.c',
  'utils.c',
  'wireless-security.c',
  'ws-leap.c',
  'ws-dynamic-wep.c',
  'ws-wep-key.c',
  'ws-wpa-eap.c',
  'ws-wpa-psk.c'
)

resource_data = files(
  'eap-method-fast.ui',
  'eap-method-leap.ui',
  'eap-method-peap.ui',
  'eap-method-simple.ui',
  'eap-method-tls.ui',
  'eap-method-ttls.ui',
  'ws-dynamic-wep.ui',
  'ws-leap.ui',
  'ws-wep-key.ui',
  'ws-wpa-eap.ui',
  'ws-wpa-psk.ui'
)

sources += gnome.compile_resources(
  name + '-resources',
  name + '.gresource.xml',
  source_dir: '.',
  c_name: name.underscorify(),
  dependencies: resource_data,
  export: true
)

# FIXME: is this uidir from ./panels/sharing/Makefile.am?
cflags = '-DUIDIR="@0@"'.format(control_center_pkgdatadir, 'ui', 'sharing')

libwireless_security = static_library(
  name,
  sources: sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)
