project('subtext', ['c', 'cpp'],
  default_options: ['buildtype=release', 'warning_level=2', 'b_lto=true', 'b_ndebug=if-release', 'cpp_std=c++14'],
  meson_version: '>=0.51.0',
  version: '2.1'
)

vapoursynth_dep = dependency('vapoursynth').partial_dependency(compile_args: true, includes: true)

deps = [
  vapoursynth_dep,
  dependency('iconv', static: get_option('static')),
  dependency('libavcodec', static: get_option('static')),
  dependency('libavformat', static: get_option('static')),
  dependency('libavutil', static: get_option('static')),
  dependency('libass', static: get_option('static')),
]

install_dir = vapoursynth_dep.get_variable(pkgconfig: 'libdir') / 'vapoursynth'

sources = [
  'src/common.c',
  'src/image.cpp',
  'src/text.c',
  'src/toass.cpp',
  'src/toutf8.c'
]

if host_machine.cpu_family().startswith('x86')
  add_project_arguments('-mfpmath=sse', '-msse2', language: 'cpp')
endif

if host_machine.system() == 'windows'
  add_project_link_arguments('-lws2_32', language: 'cpp')
endif

if get_option('static')
  add_project_link_arguments('-static', language: 'cpp')
endif

shared_module('subtext', sources,
  dependencies: deps,
  install: true,
  install_dir: install_dir,
  gnu_symbol_visibility: 'hidden'
)