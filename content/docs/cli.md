# CLI Usage

This document demonstrates how to use the KUTTL CLI

<h2>Table of Contents</h2>

[[toc]]

## Setup the KUTTL Kubectl Plugin

### Requirements

- `kubectl` version `1.13.0` or newer

### Installation

You can either download CLI binaries for linux or MacOS from our [release page](https://github.com/kudobuilder/kuttl/releases), or install the CLI plugin using `brew`:

```bash
brew tap kudobuilder/tap
brew install kuttl-cli
```

or you can compile and install the plugin from your `$GOPATH/src/github.com/kudobuilder/kuttl` root folder via:

```bash
make cli-install
```

Another alternative is [`krew`](https://github.com/kubernetes-sigs/krew), the package manager for kubectl plugins.

```bash
kubectl krew install kuttl
```

## Commands

::: flag kubectl kuttl help [command] [flags]
Provides general help or help on a specific command
:::

::: flag kubectl kuttl version
Print the current KUTTL version.
:::

::: flag kubectl kuttl test
Run KUTTL test harness.
:::



## Flags

::: tip Usage
`kubectl kuttl test <name> [flags]`
:::

::: flag -h, --help
Help for test
:::

::: flag --artifacts-dir (string)
Directory to output kind logs to (if not specified, the current working directory).
:::

::: flag --config (string)
Path to file to load test settings from. This is usually the kuttl-test.yaml file.
:::

::: flag --crd-dir (string)
Directory to load CustomResourceDefinitions from prior to running the tests.
:::

::: flag --kind-config (string)
Specify the KIND configuration file path (implies --start-kind, cannot be used with --start-control-plane).
:::

::: flag --kind-context (string)
Specify the KIND context name to use (default: kind).
:::

::: flag --manifest-dir (stringArray)
One or more directories containing manifests to apply before running the tests.
:::

::: flag --parallel (int)
The maximum number of tests to run at once. (default 8)
:::

::: flag --skip-cluster-delete (bool)
If set, do not delete the mocked control plane or kind cluster.
:::

::: flag --skip-delete (bool)
If set, do not delete resources created during tests (helpful for debugging test failures, implies --skip-cluster-delete).
:::

::: flag --start-control-plane (bool)
Start a local Kubernetes control plane for the tests (requires etcd and kube-apiserver binaries, cannot be used with --start-kind).
:::

::: flag --start-kind (bool)
Start a KIND cluster for the tests (cannot be used with --start-control-plane).
:::

::: flag --test (string)
If set, the specific test case to run.
:::


## Examples

### KUTTL Test

KUTTL test command is the heart of the test harness.  It requires a kuttl-test.yaml which defines the test setup.

```yaml
apiVersion: kuttl.dev/v1beta1
kind: TestSuite
testDirs:
- ./test/integration
parallel: 4
```

The default can be run as follows:
```bash
kubectl kuttl test  pkg/test/test_data/
```
When running with no defined [test environment](testing/test-environments.md), the default is a preconfigured cluster defined in `$KUBECONFIG`.

To run with the mocked control plane run:
```bash
kubectl kuttl test --start-control-plane pkg/test/test_data/
```

In order to run with the full kind cluster stack, run:
```bash
kubectl kuttl test --start-kind pkg/test/test_data/
```
