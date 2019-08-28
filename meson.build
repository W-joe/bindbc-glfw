## Build file for meson

project('bindbc-glfw', 'd',
	meson_version: '>=0.46.0',
	license: 'Boost',
	version: '0.6.0',
	subproject_dir: 'subprojects',
	default_options: [
		'buildtype=release',
		],
	)

compiler_id		= meson.get_compiler('d').get_id()
project_name		= meson.project_name()
project_target		= project_name + '_' + compiler_id
project_description	= 'A dynamic binding to the OpenAL library.'
project_homepage	= 'https://github.com/BindBC/bindbc-glfw'


## Dependencies
bindbc_loader_dep = dependency('bindbc-loader_' + compiler_id,
	required: false,
	version: '>=0.2.0')

if not bindbc_loader_dep.found()
	bindbc_loader = subproject('bindbc-loader')
	bindbc_loader_dep = bindbc_loader.get_variable('bindbc_loader_dep')
endif


## Import directories
import_dir = include_directories(
	'source/',
	)

## Source Files
source_files = [
	'source/bindbc/glfw/binddynamic.d',
	'source/bindbc/glfw/bindstatic.d',
	'source/bindbc/glfw/package.d',
	'source/bindbc/glfw/types.d',
	]


## Target
target_lib = static_library(
	project_target,
	sources: source_files,
	include_directories: import_dir,
	dependencies: [bindbc_loader_dep],
	install: true,
	pic: true,
	)

install_subdir('source/bindbc/', install_dir: 'include/d/')


## Dependency
bindbc_glfw_dep = declare_dependency(
	include_directories: import_dir,
	link_with: target_lib,
	version: meson.project_version(),
	dependencies: bindbc_loader_dep,
	)


## pkg-config
pkgConf = import('pkgconfig')
pkgConf.generate(
	name: project_target,
	subdirs: 'd/',
	version: meson.project_version(),
	description: project_description,
	requires: ['bindbc-loader_' + compiler_id, project_target],
	)

