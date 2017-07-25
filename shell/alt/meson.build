sources = files('cc-window.c')

cflags = [
  '-DCC_ENABLE_ALT_CATEGORIES',
  '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)
]

libshell_alt = static_library(
  'shell_alt',
  sources: sources,
  include_directories: [top_inc, common_inc, shell_inc],
  dependencies: libgd_dep,
  c_args: cflags
)
