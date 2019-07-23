package(default_visibility = ["//visibility:public"])

load("@build_stack_rules_proto//cpp:cpp_proto_library.bzl", "cpp_proto_library")
load("@build_stack_rules_proto//cpp:cpp_grpc_library.bzl", "cpp_grpc_library")

# TODO(david.bakin): python rules don't work as written because of a
# "conflicting action" problem: the `proto_library` rule writes python (via a
# `--python_out` command line parameter) which is undocumented, and that
# conflicts with the `python_{proto|grpc}_library` rules

load("@build_stack_rules_proto//python:python_proto_library.bzl", "python_proto_library")
load("@build_stack_rules_proto//python:python_grpc_library.bzl", "python_grpc_library")


PROTOS = [
    "a",
    "b",
    "s",
]

PROTOS_WITH_SERVICES = [
    "s",
]

proto_library(
    name = "a_proto",
    srcs = ["a.proto"],
    deps = [],
)

proto_library(
    name = "b_proto",
    srcs = ["b.proto"],
    deps = [":a_proto"],
)

proto_library(
    name = "s_proto",
    srcs = ["s.proto"],
    deps = [":a_proto", ":b_proto"],
)

# Each proto has _both_ a cpp-rule and a python-rule. Each proto has _either_ a
# proto-rule or a grpc-rule, depending on whether or not it has a grpc service
# defined.

[cpp_proto_library(
    name = s + "_library",
    deps = [":" + s + "_proto"],
) for s in PROTOS if s not in PROTOS_WITH_SERVICES]

[cpp_grpc_library(
    name = s + "_library",
    has_services = True,
    deps = [":" + s + "_proto"],
) for s in PROTOS if s in PROTOS_WITH_SERVICES]

[python_proto_library(
    name = s + "_py_library",
    transitive = False,
    deps = [":" + s + "_proto"],
    verbose = 1,
) for s in PROTOS if s not in PROTOS_WITH_SERVICES]

[python_grpc_library(
    name = s + "_py_library",
    has_services = True,
    transitive = False,
    deps = [":" + s + "_proto"],
    verbose = 1,
) for s in PROTOS if s in PROTOS_WITH_SERVICES]
