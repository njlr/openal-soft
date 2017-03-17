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

other_sources = [
  'Alc/backends/alsa.c',
  'Alc/backends/jack.c',
  'Alc/backends/portaudio.c',
  'Alc/backends/oss.c',
  'Alc/backends/qsa.c',
  # 'Alc/backends/null.c',
]

macos_sources = [
  'Alc/backends/coreaudio.c',
]

linux_sources = [
  'Alc/backends/pulseaudio.c',
]

windows_sources = [
  'Alc/backends/dsound.c',
  'Alc/backends/mmdevapi.c',
  'Alc/backends/winmm.c',
]

android_sources = [
  'Alc/backends/opensl.c',
]

openbsd_sources = [
  'Alc/backends/sndio.c',
]

solaris_sources = [
  'Alc/backends/solaris.c',
]

platform_sources = macos_sources + linux_sources + windows_sources + \
                   android_sources + solaris_sources + openbsd_sources + \
                   other_sources

macos_linker_flags = [
  '-framework', 'AudioToolbox',
  '-framework', 'AudioUnit',
  '-framework', 'CoreAudio',
  '-framework', 'CoreAudioKit',
  '-framework', 'CoreServices',
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
    'Alc/**/*.c',
    'common/*.c',
    'OpenAL32/*.c',
  ],
  excludes = platform_sources + glob([
    'Alc/mixer_neon.c',
  ])),
  platform_srcs = [
    ('default', macos_sources),
    ('^macos.*', macos_sources),
    ('^linux.*', linux_sources),
    ('^windows.*', windows_sources),
    ('^android.*', android_sources),
  ],
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
