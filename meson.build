project(
  'statsite', 'c',
  version: '0.8.1',
  license: 'BSD3',
  meson_version: '>=1.3.0',
  default_options: [
    'b_lto=true',
    'c_std=gnu99',
  ],
)

conf_data = configuration_data()
conf_data.set('VERSION', meson.project_version())

cc = meson.get_compiler('c')

add_project_arguments(
  cc.get_supported_arguments('-fvisibility=hidden'),
  language: 'c'
)

libstatsite = static_library(
  'statsite',
  [
    'deps/ae/ae.c',
    'deps/inih/inih.c',
    'deps/murmurhash/MurmurHash3.c',
    'src/ascii_parser.c',
    'src/carbon.c',
    'src/cm_quantile.c',
    'src/config.c',
    'src/conn_handler.c',
    'src/counter.c',
    'src/hashmap.c',
    'src/heap.c',
    'src/hll.c',
    'src/hll_constants.c',
    'src/metrics.c',
    'src/networking.c',
    'src/radix.c',
    'src/set.c',
    'src/streaming.c',
    'src/timer.c',
  ],
  include_directories: include_directories(['src', 'deps/ae', 'deps/inih']),
  install: false,
)

libstatsite_dep = declare_dependency(
  link_with: libstatsite,
  include_directories: include_directories('src'),
  dependencies: [
    cc.find_library('m', required : false),
    cc.find_library('rt', required : false),
    dependency('threads')
  ],
)

statsite = executable(
  'statsite',
  'src/statsite.c',
  dependencies: [libstatsite_dep],
  install: true
)

configure_file(
  input : 'buildconfig.h.in',
  output : 'buildconfig.h',
  configuration : conf_data
)

install_man('man/statsite.8')

install_data(
  'sinks/__init__.py',
  'sinks/binary_sink.py',
  'sinks/cloudwatch.sh',
  'sinks/gmetric.py',
  'sinks/graphite.py',
  'sinks/http.py',
  'sinks/influxdb.py',
  'sinks/librato.py',
  'sinks/opentsdb.js',
  'sinks/statsite_json_sink.rb',
  preserve_path: true,
)

install_data(
  'statsite.conf.example',
  install_dir: join_paths(get_option('sysconfdir'), 'statsite')
)

subdir('tests')
subdir('integ')
