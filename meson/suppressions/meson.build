# Project-specific warning suppressions

if cc.get_id() == 'clang'
  c_warnings += [
    '-Wno-bad-function-cast',
    '-Wno-float-equal',
    '-Wno-implicit-fallthrough',
    '-Wno-padded',
    '-Wno-reserved-id-macro',
    '-Wno-switch-default',
    '-Wno-switch-enum',
  ]
elif cc.get_id() == 'gcc'
  c_warnings += [
    '-Wno-bad-function-cast',
    '-Wno-float-equal',
    '-Wno-inline',
    '-Wno-padded',
    '-Wno-pedantic',
    '-Wno-suggest-attribute=const',
    '-Wno-suggest-attribute=malloc',
    '-Wno-suggest-attribute=pure',
    '-Wno-switch-default',
    '-Wno-switch-enum',
    '-Wno-unsuffixed-float-constants',
  ]
elif cc.get_id() == 'msvc'
  c_warnings += [
    '/wd4061',  # enumerator in switch is not explicitly handled
    '/wd4191',  # unsafe conversion from type to type
    '/wd4514',  # unreferenced inline function has been removed
    '/wd4706',  # assignment within conditional expression
    '/wd4710',  # function not inlined
    '/wd4711',  # function selected for automatic inline expansion
    '/wd4820',  # padding added after construct
    '/wd5045',  # will insert Spectre mitigation for memory load
  ]
endif

add_project_arguments(cc.get_supported_arguments(c_warnings),
                      language: ['c', 'objc'])

if is_variable('cxx')
  if cxx.get_id() == 'clang'
    cxx_warnings += [
      '-Wno-documentation-unknown-command',
      '-Wno-old-style-cast',
      '-Wno-padded',
      '-Wno-reserved-id-macro',
      '-Wno-switch-enum',
    ]
  elif cxx.get_id() == 'gcc'
    cxx_warnings += [
      '-Wno-inline',
      '-Wno-old-style-cast',
      '-Wno-padded',
      '-Wno-suggest-attribute=const',
      '-Wno-suggest-attribute=malloc',
      '-Wno-suggest-attribute=pure',
      '-Wno-suggest-final-methods',
      '-Wno-switch-default',
      '-Wno-switch-enum',
    ]
  elif cxx.get_id() == 'msvc'
    cxx_warnings += [
      '/wd4061',  # enumerator in switch is not explicitly handled
      '/wd4355',  # 'this' used in base member initializer list
      '/wd4514',  # unreferenced inline function has been removed
      '/wd4571',  # structured exceptions (SEH) are no longer caught
      '/wd4625',  # copy constructor implicitly deleted
      '/wd4626',  # assignment operator implicitly deleted
      '/wd4710',  # function not inlined
      '/wd4711',  # function selected for automatic inline expansion
      '/wd4820',  # padding added after construct
      '/wd5026',  # move constructor implicitly deleted
      '/wd5027',  # move assignment operator implicitly deleted
      '/wd5045',  # will insert Spectre mitigation for memory load
    ]
  endif

  add_project_arguments(cxx.get_supported_arguments(cxx_warnings),
                        language: ['cpp'])
endif

if is_variable('objcc')
  objc_warnings = c_warnings + [
    '-Wno-direct-ivar-access',
  ]

  add_project_arguments(objcc.get_supported_arguments(objc_warnings),
                        language: ['objc'])
endif
