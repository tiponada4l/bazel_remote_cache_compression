This is a repository that demonstrates high memory usage when using `--experimental_remote_cache_compression`.

Add .bazelrc with appropriate remote cache configuration. For instance:

```
build --bes_results_url=https://app.buildbuddy.io/invocation/
build --bes_backend=grpcs://remote.buildbuddy.io
build --remote_cache=grpcs://remote.buildbuddy.io
build --remote_download_toplevel
build --remote_timeout=3600
build --remote_header=x-buildbuddy-api-key=<key>
```

Compare the output of:

1. `bazel clean && bazel shutdown && bazel build --memory_profile=memprof :binary && grep 'Build artifacts:heap:used' memprof`
2. `bazel clean && bazel shutdown && bazel build --experimental_remote_cache_compression --memory_profile=memprof :binary && grep 'Build artifacts:heap:used' memprof`

Observe that the reported heap usage is 3-5 times higher when using `--experimental_remote_cache_compression`.

Example output (2x each of the above commands):

```
➜  bazel_remote_cache_compression git:(main) ✗ bazel clean && bazel shutdown && bazel build --memory_profile=memprof :binary && grep 'Build artifacts:heap:used' memprof
INFO: Invocation ID: 6824ae94-dd77-490e-997b-12563b1fa49b
INFO: Starting clean (this may take a while). Consider using --async if the clean takes more than several minutes.
Starting local Bazel server and connecting to it...
INFO: Invocation ID: dbfa6221-3633-480f-a02c-c161fad7461d
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/dbfa6221-3633-480f-a02c-c161fad7461d
INFO: Analyzed target //:binary (829 packages loaded, 6953 targets configured).
INFO: Found 1 target...
Target //:binary up-to-date:
  bazel-bin/binary.sh
INFO: Elapsed time: 19.249s, Critical Path: 5.21s
INFO: 2788 processes: 11 remote cache hit, 1989 internal, 788 local.
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/dbfa6221-3633-480f-a02c-c161fad7461d
INFO: Build completed successfully, 2788 total actions
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/.github/workflows/tests.yml
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/LICENSE
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/README.md
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/aspect_rules_js_metadata.json
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/binding.gyp
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/..d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/acquireOpenSSL.node.d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/configureLibssh2.node.d ... (7616 fetches)
Build artifacts:heap:used:266282232
➜  bazel_remote_cache_compression git:(main) ✗ bazel clean && bazel shutdown && bazel build --memory_profile=memprof :binary && grep 'Build artifacts:heap:used' memprof
INFO: Invocation ID: 0adff5d5-1c06-43a9-aa9d-e7d7008416d1
INFO: Starting clean (this may take a while). Consider using --async if the clean takes more than several minutes.
Starting local Bazel server and connecting to it...
INFO: Invocation ID: d06b0eb6-f060-4ae1-9df1-6b5397423de5
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/d06b0eb6-f060-4ae1-9df1-6b5397423de5
INFO: Analyzed target //:binary (829 packages loaded, 6953 targets configured).
INFO: Found 1 target...
Target //:binary up-to-date:
  bazel-bin/binary.sh
INFO: Elapsed time: 19.346s, Critical Path: 4.82s
INFO: 2788 processes: 11 remote cache hit, 1989 internal, 788 local.
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/d06b0eb6-f060-4ae1-9df1-6b5397423de5
INFO: Build completed successfully, 2788 total actions
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/.github/workflows/tests.yml
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/LICENSE
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/README.md
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/aspect_rules_js_metadata.json
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/binding.gyp
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/..d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/acquireOpenSSL.node.d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/configureLibssh2.node.d ... (8557 fetches)
Build artifacts:heap:used:252599528
➜  bazel_remote_cache_compression git:(main) ✗ bazel clean && bazel shutdown && bazel build --experimental_remote_cache_compression --memory_profile=memprof :binary && grep 'Build artifacts:heap:used' memprof
INFO: Invocation ID: 75d532b2-4a28-4b4d-9e05-e1ebaf820fe9
INFO: Starting clean (this may take a while). Consider using --async if the clean takes more than several minutes.
Starting local Bazel server and connecting to it...
INFO: Invocation ID: 6183c2b8-a4c8-4c58-8983-53836b2ace84
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/6183c2b8-a4c8-4c58-8983-53836b2ace84
INFO: Analyzed target //:binary (829 packages loaded, 6953 targets configured).
INFO: Found 1 target...
Target //:binary up-to-date:
  bazel-bin/binary.sh
INFO: Elapsed time: 19.461s, Critical Path: 4.71s
INFO: 2788 processes: 11 remote cache hit, 1989 internal, 788 local.
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/6183c2b8-a4c8-4c58-8983-53836b2ace84
INFO: Build completed successfully, 2788 total actions
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/.github/workflows/tests.yml
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/LICENSE
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/README.md
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/aspect_rules_js_metadata.json
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/binding.gyp
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/..d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/acquireOpenSSL.node.d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/configureLibssh2.node.d ... (9321 fetches)
Build artifacts:heap:used:884569976
➜  bazel_remote_cache_compression git:(main) ✗ bazel clean && bazel shutdown && bazel build --experimental_remote_cache_compression --memory_profile=memprof :binary && grep 'Build artifacts:heap:used' memprof
INFO: Invocation ID: 2ae4a65a-7ad2-49a1-9bf8-adb202cb0482
INFO: Starting clean (this may take a while). Consider using --async if the clean takes more than several minutes.
Starting local Bazel server and connecting to it...
INFO: Invocation ID: e093aa0a-b1f0-4d83-9206-69aa66c17c37
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/e093aa0a-b1f0-4d83-9206-69aa66c17c37
INFO: Analyzed target //:binary (829 packages loaded, 6953 targets configured).
INFO: Found 1 target...
Target //:binary up-to-date:
  bazel-bin/binary.sh
INFO: Elapsed time: 20.046s, Critical Path: 5.57s
INFO: 2788 processes: 11 remote cache hit, 1989 internal, 788 local.
INFO: Streaming build results to: https://app.buildbuddy.io/invocation/e093aa0a-b1f0-4d83-9206-69aa66c17c37
INFO: Build completed successfully, 2788 total actions
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/.github/workflows/tests.yml
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/LICENSE
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/README.md
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/aspect_rules_js_metadata.json
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/binding.gyp
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/..d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/acquireOpenSSL.node.d
    Fetching bazel-out/darwin_arm64-fastbuild/bin/node_modules/.aspect_rules_js/@figma+nodegit@0.28.0-figma.3/node_modules/@figma/nodegit/build/Release/.deps/Release/configureLibssh2.node.d ... (10600 fetches)
Build artifacts:heap:used:817446488
```
