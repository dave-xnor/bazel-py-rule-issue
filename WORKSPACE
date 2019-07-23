workspace(name = "bazel_py_rule_issues")

###
# Would be nice to structure this more by using bazel macros in .bzl files, but
# you can't defer much of this to a subfile from a WORKSPACE file due to
# `--incompatible_bzl_disallow_load_after_statement`. So everything is inline.

###
# Stuff for loading repos; also stuff for treating pip packages as repositories

load(
    "@bazel_tools//tools/build_defs/repo:http.bzl",
    "http_archive",
    "http_file",
)
load(
    "@bazel_tools//tools/build_defs/repo:git.bzl",
    "git_repository",
)

http_archive(
    # 2018-04-16 somewhat old, consider upgrading
    name = "io_bazel_rules_python",
    sha256 = "8b32d2dbb0b0dca02e0410da81499eef8ff051dad167d6931a92579e3b2a1d48",
    strip_prefix = "rules_python-8b5d0683a7d878b28fffe464779c8a53659fc645",
    urls = ["https://github.com/bazelbuild/rules_python/archive/" +
            "8b5d0683a7d878b28fffe464779c8a53659fc645.tar.gz"],
)

load("@io_bazel_rules_python//python:pip.bzl", "pip_import", "pip_repositories")

pip_repositories()

###
# Add rules for handling grpc, including just compiling protobufs with grpc
# enabled.
# https://github.com/stackb - Modern bazel build rules for protobuf / gRPC
# The maintained successor to https://github.com/pubref/rules_protobuf
# by same author/committer - "pre-release status"
http_archive(
    # 2019-06-03 LKG
    name = "build_stack_rules_proto",
    sha256 = "c62f0b442e82a6152fcd5b1c0b7c4028233a9e314078952b6b04253421d56d61",
    strip_prefix = "rules_proto-b93b544f851fdcd3fc5c3d47aee3b7ca158a8841",
    urls = ["https://github.com/stackb/rules_proto/archive/" +
            "b93b544f851fdcd3fc5c3d47aee3b7ca158a8841.tar.gz"],
)

load(
    "@build_stack_rules_proto//cpp:deps.bzl",
    "cpp_grpc_compile",
    "cpp_grpc_library",
    "cpp_proto_compile",
    "cpp_proto_library",
)
load(
    "@build_stack_rules_proto//python:deps.bzl",
    "python_grpc_compile",
    "python_grpc_library",
    "python_proto_compile",
    "python_proto_library",
)

pip_import(
    name = "protobuf_py_deps",
    requirements = "@build_stack_rules_proto//python/requirements:protobuf.txt",
)

load(
    "@protobuf_py_deps//:requirements.bzl",
    protobuf_pip_install = "pip_install",
)

pip_import(
    name = "grpc_py_deps",
    requirements = "@build_stack_rules_proto//python:requirements.txt",
)

load("@grpc_py_deps//:requirements.bzl", grpc_pip_install = "pip_install")

cpp_proto_compile()

cpp_grpc_compile()

cpp_proto_library()

cpp_grpc_library()

python_proto_compile()

python_grpc_compile()

python_proto_library()

python_grpc_library()

protobuf_pip_install()

grpc_pip_install()

###
# Get grpc dependencies
load("@build_stack_rules_proto//:deps.bzl", "com_github_madler_zlib", "external_zlib")

com_github_madler_zlib()  # github.com/madler/zlib is the officially maintained zlib

external_zlib()

###
# Get grpc itself

http_archive(
    name = "com_github_grpc_grpc",
    sha256 = "11ac793c562143d52fd440f6549588712badc79211cdc8c509b183cb69bddad8",
    strip_prefix = "grpc-1.22.0",
    urls = ["https://github.com/grpc/grpc/archive/v1.22.0.tar.gz"],
)

http_archive(
    name = "com_github_grpc_grpcio",
    sha256 = "8805d486c6128cc0fcc8ecf16c4095d99a8693a541ef851429ab334e028a4a97",
    strip_prefix = "grpcio-1.22.0",
    urls = ["https://files.pythonhosted.org/packages/19/c1/" +
            "bee35b6efcace3c77cb275c6465ba9e574d01acf9abf785253fdeed526f3/" +
            "grpcio-1.22.0.tar.gz"],
)

load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()
