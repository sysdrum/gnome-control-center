completion_conf = configuration_data()
completion_conf.set('PANELS', ' '.join(panel_list))

desktop_in = configure_file(
  input: meson.project_name() + '.in',
  output: meson.project_name(),
  configuration: completion_conf,
  install: true,
  install_dir: join_paths(control_center_datadir, 'bash-completion', 'completions')
)
