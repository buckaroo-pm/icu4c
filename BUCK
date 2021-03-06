prebuilt_cxx_library(
  name = 'dl',
  header_only = True,
  exported_linker_flags = [
    '-ldl',
  ],
  visibility = [
    'PUBLIC',
  ],
)

remote_file(
  name = 'icu4c-data-linux',
  out = 'icu4c-data.zip',
  type = 'exploded_zip',
  url = 'https://github.com/nikhedonia/icu4c.S/archive/ca722d76845bf951fad0a82e4dd3a06805aa009c.zip',
  sha1 = 'eb0f27b44dabc9035444fef8b0ee343027abb7d3',
)

genrule(
  name = 'icudt59l_dat-linux',
  srcs = [
    ':icu4c-data-linux'
  ],
  out = 'icudt59l_dat.S',
  cmd = 'cp $(location :icu4c-data-linux)/icu4c.S-ca722d76845bf951fad0a82e4dd3a06805aa009c/icudt59l_dat.S $OUT'
)

remote_file(
  name = 'icu4c-data-macos',
  out = 'icu4c-data.zip',
  type = 'exploded_zip',
  url = 'https://github.com/njlr/icu4c.S/archive/a73ea552920b0a72891012e124ca5ea7a74409e0.zip',
  sha1 = 'c3f0248d319b8322d2e725510ca6b4a327ee1471',
)

genrule(
  name = 'icudt59l_dat-macos',
  srcs = [
    ':icu4c-data-macos'
  ],
  out = 'icudt59l_dat.S',
  cmd = 'cp $(location :icu4c-data-macos)/icu4c.S-a73ea552920b0a72891012e124ca5ea7a74409e0/icudt59l_dat.S $OUT'
)

cxx_library(
  name = 'icu4c',
  header_namespace = '',
  exported_headers = subdir_glob([
     ('source/io', 'unicode/**/*.h'),
     ('source/data', 'unicode/**/*.h'),
     ('source/extra', 'unicode/**/*.h'),
     ('source/i18n', 'unicode/**/*.h'),
     ('source/common', 'unicode/**/*.h'),
  ]),
  headers = subdir_glob([
    ('source/io', '**/*.h'),
    ('source/data', '**/*.h'),
    ('source/extra', '**/*.h'),
    ('source/i18n', '**/*.h'),
    ('source/common', '**/*.h'),
    ('source/common/unicode', '**/*.h'),
    ('source', '**/*.h'),
  ], excludes = glob([
    'source/samples/**/*.h',
  ])),
  srcs = glob([
    'source/data/**/*.cpp',
    'source/io/**/*.cpp',
    'source/i18n/**/*.cpp',
    'source/common/**/*.cpp',
  ], excludes = glob([
    'source/**/*test.cpp',
    'source/samples/**/*.cpp',
  ])),
  platform_srcs = [
    ('macos.*', [ ':icudt59l_dat-macos' ]),
    ('linux.*', [ ':icudt59l_dat-linux' ]),
  ],
  preprocessor_flags = [
    '-DUNISTR_FROM_CHAR_EXPLICIT=explicit',
    '-DUNISTR_FROM_STRING_EXPLICIT=explicit',
    '-DU_ENABLE_DYLOAD=1',
    '-DHAVE_DLFCN_H=1',
    '-DHAVE_DLOPEN=1',
    '-DU_IO_IMPLEMENTATION',
    '-DU_I18N_IMPLEMENTATION',
    '-DU_COMMON_IMPLEMENTATION',
  ],
  platform_deps = [
    ('linux*', [ ':dl' ]),
  ],
  visibility = [
    'PUBLIC',
  ],
)
