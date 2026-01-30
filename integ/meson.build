py = import('python').find_installation(pure: false)

foreach mod : ['pytest', 'six', 'requests']
  if run_command(py, '-c', 'import ' + mod, check: false).returncode() != 0
    error('Missing Python module: ' + mod + ' (used for integ tests)')
  endif
endforeach

test_files = [
  'test_binary.py',
  'test_stdin.py',
  'test_timers_include.py',
  'test_binary_stream.py',
  'test_extended_counters.py',
  'test_http.py',
  'test_integ.py',
  'test_librato.py',
]

foreach test_file : test_files
  test(
    test_file,
    py,
    env: {
      'STATSITE': statsite.full_path(),
    },
    args:['-m', 'pytest', meson.current_source_dir() / test_file]
  )
endforeach
