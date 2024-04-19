# SPDX-License-Identifier: CC0-1.0

project('libcamera-cam', 'c', 'cpp',
    meson_version : '>= 0.60',
    version : '0.2.0',
    default_options : [
        'werror=true',
        'warning_level=2',
        'cpp_std=c++17',
    ],
    license : 'LGPL 2.1+')

apps_cpp_args = []

cc = meson.get_compiler('c')

libcamera = dependency('libcamera', required : true)

libatomic = cc.find_library('atomic', required : true)
libyaml = dependency('yaml-0.1', required : true)

libevent = dependency('libevent_pthreads', required : true)
libtiff = dependency('libtiff-4', required : false)

apps_sources = files([
    'image.cpp',
    'options.cpp',
    'ppm_writer.cpp',
    'stream_options.cpp',
])

if libevent.found()
    apps_sources += files([
        'event_loop.cpp',
    ])
endif

if libtiff.found()
    apps_cpp_args += ['-DHAVE_TIFF']
    apps_sources += files([
        'dng_writer.cpp',
    ])
endif

cam_enabled = true

cam_sources = files([
    'camera_session.cpp',
    'capture_script.cpp',
    'file_sink.cpp',
    'frame_sink.cpp',
    'main.cpp',
])

cam_cpp_args = [apps_cpp_args]

libdrm = dependency('libdrm', required : false)
libjpeg = dependency('libjpeg', required : false)
libsdl2 = dependency('SDL2', required : false)

if libdrm.found()
    cam_cpp_args += [ '-DHAVE_KMS' ]
    cam_sources += files([
        'drm.cpp',
        'kms_sink.cpp'
    ])
endif

if libsdl2.found()
    cam_cpp_args += ['-DHAVE_SDL']
    cam_sources += files([
        'sdl_sink.cpp',
        'sdl_texture.cpp',
        'sdl_texture_yuv.cpp',
    ])

    if libjpeg.found()
        cam_cpp_args += ['-DHAVE_LIBJPEG']
        cam_sources += files([
            'sdl_texture_mjpg.cpp'
        ])
    endif
endif

cam  = executable('cam', cam_sources + apps_sources,
                  dependencies : [
                      libatomic,
                      libcamera,
                      libdrm,
                      libevent,
                      libjpeg,
                      libsdl2,
                      libtiff,
                      libyaml,
                  ],
                  cpp_args : cam_cpp_args,
                  install : true,
                  install_tag : 'bin')
