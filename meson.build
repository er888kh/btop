project('btop', 'cpp',
	version: '1.2.8',
	default_options: ['cpp_std=c++20', 'warning_level=3'])

src_files = files([
	'src/btop_config.cpp',
	'src/btop_draw.cpp',
	'src/btop_input.cpp',
	'src/btop_menu.cpp',
	'src/btop_shared.cpp',
	'src/btop_theme.cpp',
	'src/btop_tools.cpp',
	'src/btop.cpp',
])

add_global_arguments('-DMESON_BUILD', language: ['cpp', 'c'])

config_data = configuration_data()
config_data.set_quoted('CONFIG_BTOP_VERSION', meson.project_version())

build_target = get_option('build_target')

platform_files_list = []
if build_target == 'linux'
	platform_files_list += 'src/linux/btop_collect.cpp'
elif build_target == 'freebsd'
	platform_files_list += 'src/freebsd/btop_collect.cpp'
else
	platform_files_list += [
		'src/osx/btop_collect.cpp',
		'src/osx/sensors.cpp',
		'src/osx/smc.cpp',
	]
endif

platform_files = files(platform_files_list)

configure_file(
	output: 'config.h',
	configuration: config_data,
)

src_inc = include_directories(['src', 'src/osx', 'src/linux', 'src/freebsd'])
thirdparty_inc = include_directories(['include'])

threads_dep = dependency('threads')

install_data('README.md', install_dir : get_option('datadir') / 'btop')
install_subdir('themes', install_dir: get_option('datadir') / 'btop')

exe = executable(
	'btop',
	sources: [src_files, platform_files],
	dependencies: [threads_dep],
	include_directories: [src_inc, thirdparty_inc],
	install: true,
)