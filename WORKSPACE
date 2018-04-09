# The WORKSPACE file tells Bazel that this directory is a "workspace", which is like a project root.
# The content of this file specifies all the external dependencies Bazel needs to perform a build.

####################################
# ESModule imports (and TypeScript imports) can be absolute starting with the workspace name.
# The name of the workspace should match the npm package where we publish, so that these
# imports also make sense when referencing the published package.
workspace(name = "ngx_tabs_libdemo")


####################################
# Fetch and install the NodeJS rules
http_archive(
  name = "build_bazel_rules_nodejs",
  url = "https://github.com/bazelbuild/rules_nodejs/archive/0.7.0.zip",
  strip_prefix = "rules_nodejs-0.7.0",
  sha256 = "d0cecf6b149d431ee8349f683d1db6a2a881ee81d8066a66c1b112a4b02748de",
)

load("@build_bazel_rules_nodejs//:defs.bzl", "check_bazel_version", "node_repositories")
check_bazel_version("0.9.0")

# NOTE: this rule installs nodejs, npm, and yarn, but does NOT install
# your npm dependencies. You must still run the package manager.
node_repositories(package_json = ["//:package.json"])

####################################
# Fetch and install the TypeScript rules
http_archive(
  name = "build_bazel_rules_typescript",
  url = "https://github.com/bazelbuild/rules_typescript/archive/0.12.0.zip",
  strip_prefix = "rules_typescript-0.12.0",
  sha256 = "60c17e558fdcb66783f39418c8dfd4858c8fd748089e42ddf91e75a853471244",
)

load("@build_bazel_rules_typescript//:defs.bzl", "ts_setup_workspace")
ts_setup_workspace()


####################################
# The Bazel buildtools repo contains tools like the BUILD file formatter, buildifier
http_archive(
    name = "com_github_bazelbuild_buildtools",
    # Note, this commit matches the version of buildifier in angular/ngcontainer
    url = "https://github.com/bazelbuild/buildtools/archive/b3b620e8bcff18ed3378cd3f35ebeb7016d71f71.zip",
    strip_prefix = "buildtools-b3b620e8bcff18ed3378cd3f35ebeb7016d71f71",
    sha256 = "dad19224258ed67cbdbae9b7befb785c3b966e5a33b04b3ce58ddb7824b97d73",
)


# Some of the TypeScript is written in Go.
# Bazel doesn't support transitive WORKSPACE deps, so we must repeat them here.
http_archive(
    name = "io_bazel_rules_go",
    url = "https://github.com/bazelbuild/rules_go/releases/download/0.9.0/rules_go-0.9.0.tar.gz",
    sha256 = "4d8d6244320dd751590f9100cf39fd7a4b75cd901e1f3ffdfd6f048328883695",
)

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()

go_register_toolchains()


####################################
# Tell Bazel about some workspaces that were installed from npm.
local_repository(
    name = "angular",
    path = "node_modules/@angular/bazel",
)

local_repository(
    name = "rxjs",
    path = "node_modules/rxjs/src",
)
