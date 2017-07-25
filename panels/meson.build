panel_list = []

panels = [
  'common',
  'background',
  'power',
  'color',
  'display',
  'mouse',
  'notifications',
  'online-accounts',
  'region',
  'info',
  'sound',
  'keyboard',
  'universal-access',
  'user-accounts',
  'datetime',
  'search',
  'privacy',
  'sharing',
  'printers'
]

if have_wacom
  panels += 'wacom'
endif

if have_network_manager
  panels += 'network'
endif

if have_bluetooth
  panels += 'bluetooth'
endif

foreach cappletname: panels
  cflags = [
    '-DG_LOG_DOMAIN="@0@-cc-panel"'.format(cappletname),
    '-DPANEL_ID="@0@"'.format(cappletname)
  ]

  subdir(cappletname)
endforeach
