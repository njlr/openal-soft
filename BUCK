def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

MACOS_CONFIG_H = """
#define AL_API  __attribute__((visibility(\\"default\\")))
#define ALC_API __attribute__((visibility(\\"default\\")))
#define ALSOFT_VERSION \\"1.17.2\\"
#define ALIGN(x) __attribute__((aligned(x)))
#define HAVE_POSIX_MEMALIGN
#define HAVE_SSE
#define HAVE_SSE2
#define HAVE_SSE3
#define HAVE_SSE4_1
#define HAVE_COREAUDIO
#define HAVE_WAVE
#define HAVE_STAT
#define HAVE_LRINTF
#define HAVE_MODFF
#define HAVE_STRTOF
#define HAVE_STRNLEN
#define SIZEOF_LONG 8
#define SIZEOF_LONG_LONG 8
#define HAVE_C99_VLA
#define HAVE_C99_BOOL
#define HAVE_C11_STATIC_ASSERT
#define HAVE_C11_ALIGNAS
//#define HAVE_C11_ATOMIC
#define HAVE_GCC_DESTRUCTOR
#define HAVE_GCC_FORMAT
#define HAVE_STDINT_H
#define HAVE_STDBOOL_H
#define HAVE_STDALIGN_H
#define HAVE_DLFCN_H
#define HAVE_DIRENT_H
#define HAVE_STRINGS_H
#define HAVE_CPUID_H
#define HAVE_FLOAT_H
#define HAVE_FENV_H
#define HAVE_GCC_GET_CPUID
#define HAVE_PTHREAD_SETSCHEDPARAM
#define HAVE_PTHREAD_SETNAME_NP
#define PTHREAD_SETNAME_NP_ONE_PARAM
"""

macos_linker_flags = [
  '-framework', 'AudioToolbox',
  '-framework', 'AudioUnit',
  '-framework', 'CoreAudio',
  '-framework', 'ApplicationServices',
  '-framework', 'ApplicationServices',
]

genrule(
  name = 'macos-config.h',
  out = 'config.h',
  cmd = 'echo "' + MACOS_CONFIG_H + '" > $OUT',
)

cxx_library(
  name = 'openal-soft',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('Alc', '**/*.h'),
    ('Include', '**/*.h'),
    ('Include/AL', '*.h'),
    ('OpenAL32/Include', '*.h'),
  ]),
  platform_headers = [
    ('default', { 'config.h': ':macos-config.h', }),
    ('^macos.*', { 'config.h': ':macos-config.h', }),
  ],
  headers = subdir_glob([
    ('Include', 'AL/*.h'),
  ]),
  srcs = glob([
    'Alc/backends/coreaudio.c',
    'Alc/effects/*.c',
    'Alc/*.c',
    'common/*.c',
    'OpenAL32/*.c',
  ],
  excludes = glob([
    'Alc/mixer_neon.c',
  ])),
  compiler_flags = [
    '-std=c11',
    '-O2',
    '-g',
    '-D_DEBUG',
    '-fPIC',
    '-Winline',
    '-Wall',
    '-Wextra',
    '-fvisibility=hidden',
    '-pthread',
    '-DAL_ALEXT_PROTOTYPES',
    '-DAL_BUILD_LIBRARY',
    '-D_LARGEFILE_SOURCE',
    '-D_LARGE_FILES',
    '-Dopenal_EXPORTS',
    '-DALSOFT_BACKEND_COREAUDIO',
    '-DALSOFT_BACKEND_WAVE',
  ],
  platform_linker_flags = [
    ('default', macos_linker_flags),
    ('^macos.*', macos_linker_flags),
  ],
  exported_platform_linker_flags = [
    ('default', macos_linker_flags),
    ('^macos.*', macos_linker_flags),
  ],
  visibility = [
    'PUBLIC',
  ],
)
