# General code to enable approximately all warnings.  This enables almost all
# warnings except those that are only relevant for very old languages (earlier
# than C99 or C++11) or non-standard extensions.
 
clang_cxx_warnings = [
  '-Wno-c++98-compat',
  '-Wno-c++98-compat-pedantic',
]

# GCC does not have -Weverything so we need to emulate it manually by enabling
# everything reasonable that is not enabled by Wall or Wextra

gcc_common_warnings = [
  # '-Wabi=',
  # '-Waggregate-return',
  # '-Walloc-size-larger-than=',
  '-Walloc-zero',
  '-Walloca',
  # '-Walloca-larger-than=0',
  '-Wanalyzer-too-complex',
  '-Warith-conversion',
  '-Warray-bounds=2',
  '-Wattribute-alias=2',
  '-Wcast-align=strict',
  '-Wcast-qual',
  '-Wconversion',
  '-Wdate-time',
  '-Wdisabled-optimization',
  '-Wdouble-promotion',
  '-Wduplicated-branches',
  '-Wduplicated-cond',
  '-Wfloat-equal',
  '-Wformat-overflow=2',
  '-Wformat-signedness',
  '-Wformat-truncation=2',
  '-Wformat=2',
  # '-Wframe-larger-than=4096',
  '-Wimplicit-fallthrough=2',
  '-Winit-self',
  '-Winline',
  '-Winvalid-pch',
  # '-Wlarger-than=4096',
  '-Wlogical-op',
  '-Wmissing-declarations',
  '-Wmissing-include-dirs',
  '-Wmultichar',
  '-Wnormalized=nfc',
  '-Wnull-dereference',
  '-Wpacked',
  '-Wpadded',
  '-Wredundant-decls',
  '-Wscalar-storage-order',
  '-Wshadow',
  '-Wshift-overflow=2',
  '-Wsizeof-array-argument',
  '-Wstack-protector',
  # '-Wstack-usage=32768',
  '-Wstrict-aliasing=3',
  '-Wstrict-overflow=5',
  '-Wstringop-overflow=3',
  '-Wsuggest-attribute=cold',
  '-Wsuggest-attribute=const',
  '-Wsuggest-attribute=format',
  '-Wsuggest-attribute=malloc',
  '-Wsuggest-attribute=noreturn',
  '-Wsuggest-attribute=pure',
  '-Wswitch-default',
  '-Wswitch-enum',
  '-Wsync-nand',
  # '-Wsystem-headers',
  # '-Wtraditional',
  # '-Wtraditional-conversion',
  # '-Wtrampolines',
  '-Wundef',
  '-Wunused-const-variable=2',
  '-Wunused-macros',
  '-Wvarargs',
  '-Wvector-operation-performance',
  '-Wvla',
  # '-Wvla-larger-than=0',
  '-Wwrite-strings',
]

gcc_c_warnings = [
  '-Wbad-function-cast',
  '-Wc++-compat',
  # '-Wc90-c99-compat',
  '-Wc99-c11-compat',
  # '-Wdeclaration-after-statement',
  '-Wdesignated-init',
  '-Wdiscarded-array-qualifiers',
  '-Wdiscarded-qualifiers',
  '-Wincompatible-pointer-types',
  '-Wjump-misses-init',
  '-Wmissing-prototypes',
  '-Wnested-externs',
  '-Wold-style-definition',
  '-Wstrict-prototypes',
  # '-Wtraditional',
  # '-Wtraditional-conversion',
  # '-Wunsuffixed-float-constants',
]

gcc_cxx_warnings = [
  '-Wabi-tag',
  '-Waligned-new=all',
  '-Wcatch-value=3',
  '-Wcomma-subscript',
  '-Wconditionally-supported',
  '-Wctor-dtor-privacy',
  '-Wdeprecated-copy-dtor',
  '-Weffc++',
  '-Wextra-semi',
  '-Wmismatched-tags',
  '-Wmultiple-inheritance',
  # '-Wnamespaces',
  '-Wnoexcept',
  '-Wnoexcept-type',
  # '-Wnon-virtual-dtor', # Enabled by Weffc++
  '-Wold-style-cast',
  '-Woverloaded-virtual',
  '-Wplacement-new=2',
  '-Wredundant-tags',
  '-Wregister',
  '-Wsign-promo',
  '-Wstrict-null-sentinel',
  '-Wsuggest-final-methods',
  '-Wsuggest-final-types',
  '-Wsuggest-override',
  # '-Wtemplates',
  '-Wuseless-cast',
  '-Wvirtual-inheritance',
  '-Wvolatile',
  '-Wzero-as-null-pointer-constant',
]

# Select C warnings
if is_variable('cc')
  c_warnings = []
  if cc.get_id() == 'clang'
    c_warnings += ['-Weverything']
  elif cc.get_id() == 'gcc'
    c_warnings += gcc_common_warnings + gcc_c_warnings
  elif cc.get_id() == 'msvc'
    c_warnings += ['/Wall']
  endif
endif

# Select C++ warnings
if is_variable('cxx')
  cxx_warnings = []
  if cxx.get_id() == 'clang'
    cxx_warnings += ['-Weverything'] + clang_cxx_warnings
  elif cxx.get_id() == 'gcc'
    cxx_warnings += gcc_common_warnings + gcc_cxx_warnings
  elif cxx.get_id() == 'msvc'
    cxx_warnings += ['/Wall']
  endif
endif
