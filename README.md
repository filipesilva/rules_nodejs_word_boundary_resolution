
Repro for https://github.com/bazelbuild/rules_nodejs/issues/313


Steps to reproduce:
- clone, install, and test
```
git clone rules_nodejs_word_boundary_resolution
cd rules_nodejs_word_boundary_resolution
yarn
yarn test
```
- you should see
```
~/s/rules_nodejs_word_boundary_resolution [1] $ yarn test
yarn run v1.21.1
$ bazelisk test //...
INFO: Invocation ID: 2609bb12-5e3e-44be-8e11-67aa51393e7a
INFO: Analyzed 4 targets (2 packages loaded, 12 targets configured).
INFO: Found 3 targets and 1 test target...
INFO: Elapsed time: 0.801s, Critical Path: 0.60s
INFO: 3 processes: 1 linux-sandbox, 2 worker.
INFO: Build completed successfully, 9 total actions
//src/name:name_test                                                     PASSED in 0.1s

Executed 1 out of 1 test: 1 test passes.
INFO: Build completed successfully, 9 total actions
Done in 0.89s.
```
- uncomment the commented code in `src/name/index.ts`
```
// import { NameSomething } from '../name-something';
// console.log(NameSomething)

import { NotNameSomething } from '../not-name-something';
console.log(NotNameSomething)

import { NameDASHSomething } from '../nameDASHsomething';
console.log(NameDASHSomething)
```
- you'll see a module resolution failure
```
~/s/rules_nodejs_word_boundary_resolution (master|…) $ yarn test
yarn run v1.21.1
$ bazelisk test //...
INFO: Invocation ID: e09ccebe-40cd-4971-8afe-97df385af0d5
INFO: Analyzed 4 targets (1 packages loaded, 9 targets configured).
INFO: Found 3 targets and 1 test target...
FAIL: //src/name:name_test (see /home/filipesilva/.cache/bazel/_bazel_filipesilva/3d40cf71d7c0ed31800942745a6d52b6/execroot/rules_nodejs_word_boundary_resolution/
bazel-out/k8-fastbuild/testlogs/src/name/name_test/test.log)
INFO: From Testing //src/name:name_test:
==================== Test output for //src/name:name_test:
Error: Cannot find module '@repro/repro/name-something/index'. Please verify that the package.json has a valid "main" entry
    at Function.module.constructor._resolveFilename (src/name/name_test_require_patch.js:485:17)
    at Function.Module._load (internal/modules/cjs/loader.js:687:27)
    at Module.require (internal/modules/cjs/loader.js:849:19)
    at require (internal/modules/cjs/helpers.js:74:18)
    at src/name/index.ts:1:1
    at /home/filipesilva/.cache/bazel/_bazel_filipesilva/3d40cf71d7c0ed31800942745a6d52b6/sandbox/linux-sandbox/27/execroot/rules_nodejs_word_boundary_resolution/bazel-out/k8-fastbuild/bin/src/name/name_test.sh.runfiles/rules_nodejs_word_boundary_resolution/src/name/index.js:3:17
    at Object.<anonymous> (/home/filipesilva/.cache/bazel/_bazel_filipesilva/3d40cf71d7c0ed31800942745a6d52b6/sandbox/linux-sandbox/27/execroot/rules_nodejs_word_boundary_resolution/bazel-out/k8-fastbuild/bin/src/name/name_test.sh.runfiles/rules_nodejs_word_boundary_resolution/src/name/index.js:9:3)
    at Module._compile (internal/modules/cjs/loader.js:956:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
    at Module.load (internal/modules/cjs/loader.js:812:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Module.require (internal/modules/cjs/loader.js:849:19)
    at require (internal/modules/cjs/helpers.js:74:18)
    at src/name/index_spec.ts:1:1
    at /home/filipesilva/.cache/bazel/_bazel_filipesilva/3d40cf71d7c0ed31800942745a6d52b6/sandbox/linux-sandbox/27/execroot/rules_nodejs_word_boundary_resolution/bazel-out/k8-fastbuild/bin/src/name/name_test.sh.runfiles/rules_nodejs_word_boundary_resolution/src/name/index_spec.js:3:17
    at Object.<anonymous> (/home/filipesilva/.cache/bazel/_bazel_filipesilva/3d40cf71d7c0ed31800942745a6d52b6/sandbox/linux-sandbox/27/execroot/rules_nodejs_word_boundary_resolution/bazel-out/k8-fastbuild/bin/src/name/name_test.sh.runfiles/rules_nodejs_word_boundary_resolution/src/name/index_spec.js:9:3)
    at Module._compile (internal/modules/cjs/loader.js:956:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
    at Module.load (internal/modules/cjs/loader.js:812:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Module.require (internal/modules/cjs/loader.js:849:19)
    at require (internal/modules/cjs/helpers.js:74:18)
    at node_modules/jasmine/lib/jasmine.js:89:5
    at Array.forEach (<anonymous>)
    at Jasmine.loadSpecs (node_modules/jasmine/lib/jasmine.js:88:18)
    at Jasmine.execute (node_modules/jasmine/lib/jasmine.js:259:8)
    at main (node_modules/@bazel/jasmine/jasmine_runner.js:215:11)
    at Object.<anonymous> (node_modules/@bazel/jasmine/jasmine_runner.js:237:22)
    at Module._compile (internal/modules/cjs/loader.js:956:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
    at Module.load (internal/modules/cjs/loader.js:812:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Object.<anonymous> (src/name/name_test_loader.js:32:24)
    at Module._compile (internal/modules/cjs/loader.js:956:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
    at Module.load (internal/modules/cjs/loader.js:812:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)
    at internal/main/run_main_module.js:17:11
================================================================================
INFO: Elapsed time: 0.596s, Critical Path: 0.38s
INFO: 3 processes: 2 linux-sandbox, 1 worker.
INFO: Build completed, 1 test FAILED, 3 total actions
//src/name:name_test                                                     FAILED in 0.1s
  /home/filipesilva/.cache/bazel/_bazel_filipesilva/3d40cf71d7c0ed31800942745a6d52b6/execroot/rules_nodejs_word_boundary_resolution/bazel-out/k8-fastbuild/testlogs/src/name/name_test/test.log

INFO: Build completed, 1 test FAILED, 3 total actions
error Command failed with exit code 3.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```