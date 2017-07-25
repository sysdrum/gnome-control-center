info = meson.project_name() + '.metainfo.xml'

custom_target(
  info,
  input: meson.project_name() + '.appdata.xml.in',
  output: info,
  command: intltool_xml_cmd,
  install: true,
  install_dir: join_paths(control_center_datadir, 'metainfo')
)
