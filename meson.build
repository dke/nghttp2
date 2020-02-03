project('nghttp2_asio', 'c', 'cpp',
  version : '1.41.0-DEV',
  default_options : ['cpp_std=c++14']
  )

openssl_dep = dependency('openssl')
boost_dep = dependency('boost', modules: ['thread', 'system'])

# would love to have that stuff in an include, but meson can't do includes?
jansson_dep = dependency('jansson', required : false)
neverbleed_dep = dependency('neverbleed', required : false)
libsystemd_dep = dependency('libsystemd', required : false)
if not libsystemd_dep.found()
  libsystemd_dep = dependency('systemd', required : false)
endif
libxml2_dep = dependency('libxml2', required : false)
if not libxml2_dep.found()
  # this one is not optional. fail if none of them is present
  libxml2_dep = dependency('libxml-2.0', required : false)
endif

libmruby_dep = dependency('libmruby', required : false)

conf = configuration_data()

# TODO
#conf.set('DEBUGBUILD', 1)
#/* If available, contains the Python version number currently in use. */
#/* #undef HAVE_PYTHON */
#/* Define if building universal (internal helper macro) */
#/* #undef AC_APPLE_UNIVERSAL_BUILD */
#/* Define to the sub-directory where libtool stores uninstalled libraries. */
##define LT_OBJDIR ".libs/"
#/* Define to 1 if you want to disable threads. */
#/* #undef NOTHREADS */
#/* Define to 1 if assertions should be disabled. */
#/* #undef NDEBUG */

# hardcoded:
#conf.set('HAVE_WORKING_FORK', 1)
#conf.set('HAVE_WORKING_VFORK', 1)
#/* Define to 1 if strerror_r returns char *. */
#conf.set('STRERROR_R_CHAR_P', 1)
#/* Hint to the compiler that a function never return */
#conf.set_quoted('NGHTTP2_NORETURN', '__attribute__((noreturn))')


conf.set_quoted('VERSION', meson.project_version())
conf.set_quoted('PACKAGE_VERSION', meson.project_version())
conf.set_quoted('PACKAGE', 'nghttp2')
conf.set_quoted('PACKAGE_NAME', 'nghttp2')
conf.set_quoted('PACKAGE_STRING', 'nghttp2 '+meson.project_version())
conf.set_quoted('PACKAGE_TARNAME', 'nghttp2')
conf.set_quoted('PACKAGE_URL', '')
conf.set_quoted('PACKAGE_BUGREPORT', 't-tujikawa@users.sourceforge.net')

conf.set('STDC_HEADERS', 1)
conf.set('HAVE_CXX14', 1)
conf.set_quoted('NGHTTP2_NORETURN', '__attribute__((noreturn))')
conf.set('STRERROR_R_CHAR_P', 1)

if host_machine.endian()=='big'
  conf.set('WORDS_BIGENDIAN', 1)
endif

# zsh
# for thing in arpa/inet.h fcntl.h inttypes.h limits.h netdb.h netinet/in.h pwd.h stddef.h stdint.h stdlib.h string.h sys/socket.h sys/time.h syslog.h time.h unistd.h
#   do echo "if meson.get_compiler('c').check_header('$thing')\n  conf.set('HAVE_$thing:u:s,/,_,:s,.,_,', 1)\nendif"
# done

if meson.get_compiler('c').check_header('arpa/inet.h')
  conf.set('HAVE_ARPA_INET_H', 1)
endif
if meson.get_compiler('c').check_header('fcntl.h')
  conf.set('HAVE_FCNTL_H', 1)
endif
if meson.get_compiler('c').check_header('inttypes.h')
  conf.set('HAVE_INTTYPES_H', 1)
endif
if meson.get_compiler('c').check_header('limits.h')
  conf.set('HAVE_LIMITS_H', 1)
endif
if meson.get_compiler('c').check_header('netdb.h')
  conf.set('HAVE_NETDB_H', 1)
endif
if meson.get_compiler('c').check_header('netinet/in.h')
  conf.set('HAVE_NETINET_IN_H', 1)
endif
if meson.get_compiler('c').check_header('pwd.h')
  conf.set('HAVE_PWD_H', 1)
endif
if meson.get_compiler('c').check_header('stddef.h')
  conf.set('HAVE_STDDEF_H', 1)
endif
if meson.get_compiler('c').check_header('stdint.h')
  conf.set('HAVE_STDINT_H', 1)
endif
if meson.get_compiler('c').check_header('stdlib.h')
  conf.set('HAVE_STDLIB_H', 1)
endif
if meson.get_compiler('c').check_header('string.h')
  conf.set('HAVE_STRING_H', 1)
endif
if meson.get_compiler('c').check_header('sys/socket.h')
  conf.set('HAVE_SYS_SOCKET_H', 1)
endif
if meson.get_compiler('c').check_header('sys/time.h')
  conf.set('HAVE_SYS_TIME_H', 1)
endif
if meson.get_compiler('c').check_header('syslog.h')
  conf.set('HAVE_SYSLOG_H', 1)
endif
if meson.get_compiler('c').check_header('time.h')
  conf.set('HAVE_TIME_H', 1)
endif
if meson.get_compiler('c').check_header('unistd.h')
  conf.set('HAVE_UNISTD_H', 1)
endif
if meson.get_compiler('c').check_header('dlfcn.h')
  conf.set('HAVE_DLFCN_H', 1)
endif
if meson.get_compiler('c').check_header('memory.h')
  conf.set('HAVE_MEMORY_H', 1)
endif
if meson.get_compiler('c').check_header('strings.h')
  conf.set('HAVE_STRINGS_H', 1)
endif
if meson.get_compiler('c').check_header('sys/stat.h')
  conf.set('HAVE_SYS_STAT_H', 1)
endif
if meson.get_compiler('c').check_header('sys/types.h')
  conf.set('HAVE_SYS_TYPES_H', 1)
endif
if meson.get_compiler('c').check_header('vfork.h')
  conf.set('HAVE_VFORK_H', 1)
endif

# zsh
#for thing in _Exit accept4 dup2 getcwd getpwnam localtime_r memchr memmove memset mkostemp socket sqrt strchr strdup strerror strndup strstr strtol strtoul timegm
#do
#  echo "if meson.get_compiler('c').has_function('$thing')\n  conf.set('HAVE_$thing:u', 1)\nendif"
#done

if meson.get_compiler('c').has_function('_Exit')
  conf.set('HAVE__EXIT', 1)
endif
if meson.get_compiler('c').has_function('accept4')
  conf.set('HAVE_ACCEPT4', 1)
endif
if meson.get_compiler('c').has_function('dup2')
  conf.set('HAVE_DUP2', 1)
endif
if meson.get_compiler('c').has_function('getcwd')
  conf.set('HAVE_GETCWD', 1)
endif
if meson.get_compiler('c').has_function('getpwnam')
  conf.set('HAVE_GETPWNAM', 1)
endif
if meson.get_compiler('c').has_function('localtime_r')
  conf.set('HAVE_LOCALTIME_R', 1)
endif
if meson.get_compiler('c').has_function('memchr')
  conf.set('HAVE_MEMCHR', 1)
endif
if meson.get_compiler('c').has_function('memmove')
  conf.set('HAVE_MEMMOVE', 1)
endif
if meson.get_compiler('c').has_function('memset')
  conf.set('HAVE_MEMSET', 1)
endif
if meson.get_compiler('c').has_function('mkostemp')
  conf.set('HAVE_MKOSTEMP', 1)
endif
if meson.get_compiler('c').has_function('socket')
  conf.set('HAVE_SOCKET', 1)
endif
if meson.get_compiler('c').has_function('sqrt')
  conf.set('HAVE_SQRT', 1)
endif
if meson.get_compiler('c').has_function('strchr')
  conf.set('HAVE_STRCHR', 1)
endif
if meson.get_compiler('c').has_function('strdup')
  conf.set('HAVE_STRDUP', 1)
endif
if meson.get_compiler('c').has_function('strerror')
  conf.set('HAVE_STRERROR', 1)
endif
if meson.get_compiler('c').has_function('strndup')
  conf.set('HAVE_STRNDUP', 1)
endif
if meson.get_compiler('c').has_function('strstr')
  conf.set('HAVE_STRSTR', 1)
endif
if meson.get_compiler('c').has_function('strtol')
  conf.set('HAVE_STRTOL', 1)
endif
if meson.get_compiler('c').has_function('strtoul')
  conf.set('HAVE_STRTOUL', 1)
endif
if meson.get_compiler('c').has_function('timegm')
  conf.set('HAVE_TIMEGM', 1)
endif

if meson.get_compiler('c').has_function('chown')
  conf.set('HAVE_CHOWN', 1)
endif
if meson.get_compiler('c').has_function('fork')
  conf.set('HAVE_FORK', 1)
  conf.set('HAVE_WORKING_FORK', 1)
endif
if meson.get_compiler('c').has_function('localtime_r')
  conf.set('HAVE_LOCALTIME_R', 1)
endif
if meson.get_compiler('c').has_function('strerror_r')
  conf.set('HAVE_STRERROR_R', 1)
endif
if meson.get_compiler('c').has_function('initgroups')
  conf.set('HAVE_DECL_INITGROUPS', 1)
else
  conf.set('HAVE_DECL_INITGROUPS', 0)
endif
if meson.get_compiler('c').has_function('strerror_r')
  conf.set('HAVE_DECL_STRERROR_R', 1)
else
  conf.set('HAVE_DECL_STRERROR_R', 0)
endif
if meson.get_compiler('c').has_function('vfork')
  conf.set('HAVE_VFORK', 1)
  conf.set('HAVE_WORKING_VFORK', 1)
endif

if jansson_dep.found()
  conf.set('HAVE_JANSSON', 1)
endif
if neverbleed_dep.found()
  conf.set('HAVE_NEVERBLEED', 1)
endif
if libsystemd_dep.found()
  conf.set('HAVE_LIBSYSTEMD', 1)
endif
if libxml2_dep.found()
  conf.set('HAVE_LIBXML2', 1)
endif
if libmruby_dep.found()
  conf.set('HAVE_LIBMRUBY', 1)
endif
if boost_dep.found()
  conf.set('HAVE_BOOST', true)
  conf.set('HAVE_BOOST_ASIO', true)
  conf.set('HAVE_BOOST_SYSTEM', true)
  conf.set('HAVE_BOOST_THREAD', true)
endif

if meson.get_compiler('c').has_member('struct tm', 'tm_gmtoff', prefix : '#include<time.h>')
  conf.set('HAVE_STRUCT_TM_TM_GMTOFF', 1)
endif
if meson.get_compiler('c').has_member('struct sockaddr_in6', 'sin6_len', prefix : '#include <sys/socket.h>\n#include <netinet/in.h>')
  conf.set('HAVE_SOCKADDR_IN6_SIN6_LEN', 1)
endif
if meson.get_compiler('c').has_member('struct sockaddr_in', 'sin_len', prefix : '#include <sys/socket.h>\n#include <netinet/in.h>')
  conf.set('HAVE_SOCKADDR_IN_SIN_LEN', 1)
endif

atomic_std_shared_ptr_code = '''
#include <memory>

int main () {
  auto a = std::make_shared<int>(1000000007);
  auto p = std::atomic_load(&a);
  ++*p;
  std::atomic_store(&a, p);
  ;
  return 0;
}
'''
if meson.get_compiler('cpp').compiles(atomic_std_shared_ptr_code, name: 'we have the std::atomic_* overloads for std::shared_ptr')
  conf.set('HAVE_ATOMIC_STD_SHARED_PTR', 1)
endif

thread_local_code = '''
int main () {
  thread_local int a = 0;
  (void)a;
  ;
  return 0;
}
'''
if meson.get_compiler('cpp').compiles(thread_local_code, name: 'we have thread_local storage specifier')
  conf.set('HAVE_THREAD_LOCAL', 1)
endif

std_future_code = '''
#include <vector>
#include <future>

int main () {
  std::vector<std::future<int>> v;
  ;
  return 0;
}
'''
if meson.get_compiler('cpp').compiles(std_future_code, name: 'we have std::future')
  conf.set('HAVE_STD_FUTURE', 1)
endif

std_map_emplace_code='''
#include <map>

int main () {
  std::map<int, int>().emplace(1, 2);
  ;
  return 0;
}
'''
if meson.get_compiler('cpp').compiles(std_map_emplace_code, name: 'we have std::map::emplace')
  conf.set('HAVE_STD_MAP_EMPLACE', 1)
endif

if meson.get_compiler('c').has_header_symbol('stddef.h', 'ptrdiff_t')
  conf.set('HAVE_PTRDIFF_T', 1)
endif

conf.set('SIZEOF_INT_P', meson.get_compiler('c').sizeof('int *'))
conf.set('SIZEOF_TIME_T', meson.get_compiler('c').sizeof('time_t', prefix : '#include<time.h>'))

configure_file(output : 'config.h',
               configuration : conf)

meson_project_version_array = meson.project_version().split('.')
PACKAGE_VERSION_NUM_command='printf "0x%02x%02x%02x" "@0@" "@1@" "@2@"'.format(
  meson_project_version_array[0].to_int(),
  meson_project_version_array[1].to_int(),
  meson_project_version_array[2].split('-')[0].to_int())
r = run_command('/bin/sh', '-c', PACKAGE_VERSION_NUM_command)
PACKAGE_VERSION_NUM = r.stdout().strip()

ver_conf = configuration_data()
ver_conf.set('PACKAGE_VERSION_NUM', PACKAGE_VERSION_NUM)
ver_conf.set('PACKAGE_VERSION', meson.project_version())

configure_file(input : 'lib/includes/nghttp2/nghttp2ver.h.in',
  output : 'nghttp2ver.h',
  configuration : ver_conf)

libnghttp2_compile_args = ['-DHAVE_CONFIG_H']
libnghttp2_asio_compile_args = ['-DHAVE_CONFIG_H']

libnghttp2_sources = files(
   'lib/nghttp2_pq.c',
   'lib/nghttp2_map.c',
   'lib/nghttp2_queue.c',
   'lib/nghttp2_frame.c',
   'lib/nghttp2_buf.c',
   'lib/nghttp2_stream.c',
   'lib/nghttp2_outbound_item.c',
   'lib/nghttp2_session.c',
   'lib/nghttp2_submit.c',
   'lib/nghttp2_helper.c',
   'lib/nghttp2_npn.c',
   'lib/nghttp2_hd.c',
   'lib/nghttp2_hd_huffman.c',
   'lib/nghttp2_hd_huffman_data.c',
   'lib/nghttp2_version.c',
   'lib/nghttp2_priority_spec.c',
   'lib/nghttp2_option.c',
   'lib/nghttp2_callbacks.c',
   'lib/nghttp2_mem.c',
   'lib/nghttp2_http.c',
   'lib/nghttp2_rcbuf.c',
   'lib/nghttp2_debug.c'
)
libnghttp2_include = include_directories('lib/includes')
libnghttp2_dep = declare_dependency(sources: libnghttp2_sources,
  include_directories: [libnghttp2_include],
  compile_args: libnghttp2_compile_args)
nghttp2_lib_static = static_library('nghttp2', dependencies: [libnghttp2_dep])
nghttp2_lib_static_dep = declare_dependency(link_with: nghttp2_lib_static, include_directories: [libnghttp2_include])

liburl_parser_sources = files(
  'third-party/url-parser/url_parser.c'
)
liburl_parser_include = include_directories('third-party')
liburl_parser_dep = declare_dependency(sources: liburl_parser_sources)
url_lib_static = static_library('url', dependencies: [liburl_parser_dep])
url_lib_static_dep = declare_dependency(link_with: url_lib_static)

libllhttp_sources = files(
   'third-party/llhttp/src/api.c',
   'third-party/llhttp/src/http.c',
   'third-party/llhttp/src/llhttp.c'
)
libllhttp_include = include_directories('third-party/llhttp/include')
libllhttp_dep = declare_dependency(sources: libllhttp_sources, include_directories: [libllhttp_include])
llhttp_lib_static = static_library('llhttp', dependencies: [libllhttp_dep])
llhttp_lib_static_dep = declare_dependency(link_with: llhttp_lib_static, include_directories: [libllhttp_include])

libnghttp2_asio_sources = files(
   'src/util.cc',
   'src/http2.cc',
   'src/tls.cc',
   'src/timegm.c',
   'src/asio_common.cc',
   'src/asio_io_service_pool.cc',
   'src/asio_server_http2.cc',
   'src/asio_server_http2_impl.cc',
   'src/asio_server.cc',
   'src/asio_server_http2_handler.cc',
   'src/asio_server_request.cc',
   'src/asio_server_request_impl.cc',
   'src/asio_server_response.cc',
   'src/asio_server_response_impl.cc',
   'src/asio_server_stream.cc',
   'src/asio_server_serve_mux.cc',
   'src/asio_server_request_handler.cc',
   'src/asio_server_tls_context.cc',
   'src/asio_client_session.cc',
   'src/asio_client_session_impl.cc',
   'src/asio_client_session_tcp_impl.cc',
   'src/asio_client_session_tls_impl.cc',
   'src/asio_client_response.cc',
   'src/asio_client_response_impl.cc',
   'src/asio_client_request.cc',
   'src/asio_client_request_impl.cc',
   'src/asio_client_stream.cc',
   'src/asio_client_tls_context.cc'
)
libnghttp2_asio_include = include_directories('src/includes', 'lib/includes')
libnghttp2_asio_dep = declare_dependency(sources: libnghttp2_asio_sources,
  dependencies: [boost_dep, openssl_dep, llhttp_lib_static_dep, url_lib_static_dep, nghttp2_lib_static_dep],
  include_directories: [libnghttp2_asio_include, liburl_parser_include],
  compile_args: libnghttp2_asio_compile_args)
nghttp2_asio_lib_static = static_library('nghttp2_asio', dependencies: [libnghttp2_asio_dep])
nghttp2_asio_lib_static_dep = declare_dependency(link_with: nghttp2_asio_lib_static, include_directories: [libnghttp2_asio_include])
