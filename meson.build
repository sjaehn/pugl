project('pugl', ['c'],
        version: '0.2.0',
        license: 'ISC',
        meson_version: '>= 0.53.0',
        default_options: ['c_std=c99', 'cpp_std=c++11'])

# Build tools
pkg = import('pkgconfig')
cc = meson.get_compiler('c')
if get_option('examples')
  add_languages(['cpp'])
  cxx = meson.get_compiler('cpp')
endif
if build_machine.system() == 'darwin'
  add_languages(['objc'])
  objcc = meson.get_compiler('objc')
endif

if get_option('strict') == true
  subdir('meson/warnings')
  subdir('meson/suppressions')
endif

add_project_arguments(['-DPUGL_DISABLE_DEPRECATED'],
                      language: ['c', 'cpp', 'objc'])

major_version = meson.project_version().split('.')[0]
version_suffix = '-@0@'.format(major_version)
versioned_name = 'pugl' + version_suffix

public_headers = [
  'pugl/gl.h',
  'pugl/glu.h',
  'pugl/pugl.h',
  'pugl/pugl.hpp',
  'pugl/pugl.ipp',
  'pugl/pugl_cairo.h',
  'pugl/pugl_cairo.hpp',
  'pugl/pugl_gl.h',
  'pugl/pugl_gl.hpp',
  'pugl/pugl_stub.h',
  'pugl/pugl_stub.hpp',
]

implementation_files = [
  'pugl/detail/implementation.c',
  'pugl/detail/implementation.h',
  'pugl/detail/mac.h',
  'pugl/detail/mac.m',
  'pugl/detail/mac_cairo.m',
  'pugl/detail/mac_gl.m',
  'pugl/detail/mac_stub.m',
  'pugl/detail/stub.h',
  'pugl/detail/types.h',
  'pugl/detail/win.c',
  'pugl/detail/win.h',
  'pugl/detail/win_cairo.c',
  'pugl/detail/win_gl.c',
  'pugl/detail/x11.c',
  'pugl/detail/x11.h',
  'pugl/detail/x11_cairo.c',
  'pugl/detail/x11_gl.c',
]

core_sources = [
  'pugl/detail/implementation.c'
]

gl_examples = [
  'pugl_cxx_demo.cpp',
  'pugl_embed_demo.c',
  'pugl_print_events.c',
  'pugl_shader_demo.c',
  'pugl_window_demo.c',
]

cairo_examples = [
  'pugl_cairo_demo.c'
]

gl_tests = ['gl_hints']
basic_tests = ['stub_hints', 'redisplay', 'show_hide', 'update', 'timer']
tests = ['gl_hints', 'stub_hints', 'redisplay', 'show_hide', 'update', 'timer']

# Build tools
pkg = import('pkgconfig')
cc = meson.get_compiler('c')
if get_option('examples')
  add_languages(['cpp'])
endif
if build_machine.system() == 'darwin'
  add_languages(['objc'])
  cxx = meson.get_compiler('objc')
endif

# System libraries
m_dep = cc.find_library('m', required: false)
dl_dep = cc.find_library('dl', required: false)

# Cairo
cairo_dep = disabler()
enable_cairo = get_option('cairo')
if enable_cairo != 'no'
  cairo_dep = dependency('cairo',
                         required: enable_cairo == 'yes',
                         include_type: 'system')
endif

# OpenGL
opengl_dep = disabler()
enable_opengl = get_option('opengl')
if enable_opengl != 'no'
  opengl_dep = dependency('GL',
                          required: enable_opengl == 'yes',
                          include_type: 'system')
endif

core_args = []

# Determine target platform
if build_machine.system() == 'darwin'
  add_project_arguments(['-Wno-deprecated-declarations'], language: ['objc'])
  cocoa_dep = dependency('Cocoa', required: false, modules: 'foundation')
  corevideo_dep = dependency('CoreVideo', required: false)

  platform = 'mac'
  platform_sources = ['pugl/detail/mac.m', 'pugl/detail/mac_stub.m']
  core_deps = [cocoa_dep, corevideo_dep]
  extension = '.m'

  add_project_arguments(['-DGL_SILENCE_DEPRECATION'],
                        language: ['c', 'objc'])

  add_project_link_arguments(['-Wl,-framework,Cocoa'],
                             language: ['c', 'objc'])

elif build_machine.system() == 'windows'
  platform = 'win'
  platform_sources = ['pugl/detail/win.c']
  extension = '.c'
else
  x11_dep = cc.find_library('X11')

  xcursor_dep = cc.find_library('Xcursor', required: false)
  if xcursor_dep.found()
    core_args += ['-DHAVE_XCURSOR']
  endif

  xrandr_dep = cc.find_library('Xrandr', required: false)
  if xrandr_dep.found()
    core_args += ['-DHAVE_XRANDR']
  endif

  xext_dep = cc.find_library('Xext', required: false)
  if xext_dep.found()
    xsync_fragment = '''#include <X11/Xlib.h>
      #include <X11/extensions/sync.h>
      int main(void) { XSyncQueryExtension(0, 0, 0); return 0; }'''
    if cc.compiles(xsync_fragment, name: 'Xsync')
      core_args += ['-DHAVE_XSYNC']
    endif
  endif

  platform = 'x11'
  platform_sources = ['pugl/detail/x11.c']
  core_deps = [x11_dep, xcursor_dep, xrandr_dep, xext_dep]
  extension = '.c'
endif

# Build core library

shared_args = ['-DPUGL_INTERNAL', '-DPUGL_SHARED']
static_args = ['-DPUGL_INTERNAL']
core_deps += [m_dep]
core_sources += platform_sources
core_name = 'pugl_@0@@1@'.format(platform, version_suffix)

libpugl = shared_library(core_name, core_sources,
                         version: meson.project_version(),
                         include_directories: ['.'],
                         c_args: shared_args + core_args,
                         dependencies: core_deps,
                         gnu_symbol_visibility: 'hidden',
                         install: true)

libpugl_a = static_library(core_name, core_sources,
                           include_directories: ['.'],
                           c_args: static_args + core_args,
                           dependencies: core_deps,
                           install: true)

pugl_dep = declare_dependency(link_with: libpugl, dependencies: core_deps)
pugl_a_dep = declare_dependency(link_with: libpugl_a, dependencies: core_deps)

pkg.generate([libpugl],
             libraries_private: [libpugl_a],
             name: 'Pugl',
             filebase: versioned_name,
             subdirs: [versioned_name],
             version: meson.project_version(),
             description: 'Pugl GUI library core')

# Build GL backend
if opengl_dep.found()
  name = 'pugl_' + platform + '_gl' + version_suffix
  sources = 'pugl/detail/' + platform + '_gl' + extension

  gl_backend = shared_library(name, sources,
                              version: meson.project_version(),
                              include_directories: ['.'],
                              c_args: shared_args,
                              dependencies: [pugl_dep, opengl_dep],
                              gnu_symbol_visibility: 'hidden',
                              install: true)

  gl_backend_a = static_library(name, sources,
                                include_directories: ['.'],
                                c_args: static_args,
                                dependencies: [pugl_a_dep, opengl_dep],
                                install: true)

  gl_backend_dep = declare_dependency(link_with: gl_backend_a,
                                      dependencies: [pugl_a_dep, opengl_dep])

  pkg.generate(libraries: [libpugl, gl_backend],
               libraries_private: [libpugl_a, gl_backend_a],
               name: 'Pugl OpenGL',
               filebase: 'pugl-gl-@0@'.format(major_version),
               subdirs: [name],
               version: meson.project_version(),
               description: 'Pugl GUI library with OpenGL backend')
endif

# Build Cairo backend
if cairo_dep.found()
  name = 'pugl_' + platform + '_cairo' + version_suffix
  sources = 'pugl/detail/' + platform + '_cairo' + extension

  cairo_backend = shared_library(name, sources,
                                 version: meson.project_version(),
                                 include_directories: ['.'],
                                 c_args: shared_args,
                                 dependencies: [pugl_dep, cairo_dep],
                                 gnu_symbol_visibility: 'hidden',
                                 install: true)

  cairo_backend_a = static_library(name, sources,
                                   include_directories: ['.'],
                                   c_args: static_args,
                                   dependencies: [pugl_a_dep, cairo_dep],
                                   install: true)

  cairo_backend_dep = declare_dependency(link_with: cairo_backend_a,
                                         dependencies: [pugl_a_dep, cairo_dep])

  pkg.generate(libraries: [libpugl, cairo_backend],
               libraries_private: [libpugl_a, cairo_backend_a],
               name: 'Pugl Cairo',
               filebase: 'pugl-cairo-@0@'.format(major_version),
               subdirs: [name],
               version: meson.project_version(),
               description: 'Pugl GUI library with Cairo backend')
endif

if get_option('examples')
  # Build GL examples
  if opengl_dep.found()
    foreach example : gl_examples
      source = ['examples/' + example]
      target = example.split('.')[0]
      dependencies = [gl_backend_dep]

      if target == 'pugl_shader_demo'
        source += ['examples/glad/glad.c']
        dependencies += [dl_dep]
      endif

      executable(target, source,
                 include_directories: ['.'],
                 dependencies: dependencies,
                 gui_app: true)
    endforeach
  endif

  # Build Cairo examples
  if cairo_dep.found()
    foreach example : cairo_examples
      source = ['examples/' + example]
      target = example.split('.')[0]
      executable(target, source,
                 include_directories: ['.'],
                 dependencies: [pugl_a_dep, cairo_backend_dep],
                 gui_app: true)
    endforeach
  endif
endif

if get_option('tests')
  foreach test : basic_tests
    test(test,
         executable('test_' + test, 'test/test_@0@.c'.format(test),
                    dependencies: [pugl_a_dep]))
  endforeach

  if opengl_dep.found()
    foreach test : gl_tests
      test(test,
           executable('test_' + test, 'test/test_@0@.c'.format(test),
                      dependencies: [pugl_a_dep, gl_backend_dep]))
    endforeach
  endif
endif

install_headers(public_headers, subdir: versioned_name / 'pugl')
if get_option('implementation_headers')
  install_headers(implementation_files, subdir: versioned_name / 'pugl')
endif

summary('Platform', platform)
summary('Cairo backend', cairo_dep.found(), bool_yn: true)
summary('OpenGL backend', opengl_dep.found(), bool_yn: true)
summary('Tests', get_option('tests'), bool_yn: true)
summary('Examples', get_option('examples'), bool_yn: true)
