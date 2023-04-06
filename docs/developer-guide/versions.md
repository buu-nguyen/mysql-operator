# Versions

## operator-sdk

Originally created with [v1.10.1](https://github.com/operator-framework/operator-sdk/releases/tag/v1.10.1)

`Makefile` was updated with [v1.28.0](https://github.com/operator-framework/operator-sdk/releases/tag/v1.28.0)

Steps:

1. create temporary dir
1. create operator-sdk project
    ```
    operator-sdk init --domain nakamasato.com --repo github.com/nakamasato/mysql-operator
    ```
1. copy `Makefile` to this repo
1. Update a few points
    1. IMAGE_TAG_BASE
        ```
        IMAGE_TAG_BASE ?= nakamasato/mysql-operator
        ```
    1. IMG
        ```
        IMG ?= ghcr.io/nakamasato/mysql-operator
        ```
    1. test
        ```
        test: manifests generate fmt vet envtest ## Run tests.
                KUBEBUILDER_ASSETS="$(shell $(ENVTEST) use $(ENVTEST_K8S_VERSION) -p path)" $(GINKGO) -cover -coverprofile cover.out -covermode=atomic -sk
        ip-package=e2e ./...
        ```
    1. gingko
        ```
        GINKGO_VERSION ?= v2.9.2
        ```

        ```
        GINKGO = $(LOCALBIN)/ginkgo
        ginkgo:
            test -s $(LOCALBIN)/ginkgo && $(LOCALBIN)/ginkgo --version | grep -q $(GINKGO_VERSION) || \
            GOBIN=$(LOCALBIN) go install github.com/onsi/ginkgo/v2/ginkgo@$(GINKGO_VERSION)
        ```