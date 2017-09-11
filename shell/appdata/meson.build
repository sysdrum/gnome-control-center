info = meson.project_name() + '.metainfo.xml'

i18n.merge_file(
  info,
  input: meson.project_name() + '.appdata.xml.in',
  output: info,
  po_dir: po_dir,
  install: true,
  install_dir: join_paths(control_center_datadir, 'metainfo')
)
