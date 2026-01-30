check_dep = dependency('check')

test_runner = executable(
  'runner',
  'runner.c',
  c_args: ['-Wno-int-conversion'],
  dependencies: [check_dep, libstatsite_dep],
  install: false,
)

test(
  'tests',
  test_runner,
  env: {
    'CK_FORK': 'yes',
    'CK_VERBOSITY': 'verbose',
  }
)
