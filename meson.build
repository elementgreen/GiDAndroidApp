project('ant', 'd', meson_version: '>=1.9.0')

gid_dep = dependency('gid:gtk4', method: 'dub')

dependency('gtk4')

exe = executable('ant', ['app.d'], dependencies: [
 gid_dep        
], android_exe_type: 'application', install: true, link_args: ['-mtriple=aarch64-none-linux-android', '-link-defaultlib-shared=false', '-llog', '-landroid'])
