# proto-template

Central protobuf definitions for gRPC services. Other repos (like [`grpc-template`](https://github.com/kitti12911/grpc-template)) generate code from this repo.

## requirements

- [buf](https://buf.build/) installed
- vscode with `bufbuild/buf` extension (for syntax highlight and auto format)

## install buf

- macos:

    ```bash
    brew install bufbuild/buf/buf
    ```

- linux:

    ```bash
    # see https://buf.build/docs/cli/installation for other methods
    curl -sSL https://github.com/bufbuild/buf/releases/latest/download/buf-$(uname -s)-$(uname -m) -o /usr/local/bin/buf
    chmod +x /usr/local/bin/buf
    ```

## project structure

```bash
proto-template/
├── example/
│   └── v1/
│       └── example.proto     # example CRUD service definition
├── buf.yaml                  # buf config (lint + breaking change rules)
├── Makefile
└── README.md
```

## how to add a new service

1. create a directory for your service following the pattern `<service>/<version>/`

    ```bash
    my-service/
    └── v1/
        └── my_service.proto
    ```

2. set the go package option in your proto file

    ```protobuf
    option go_package = "grpc-template/gen/my-service/v1;myservicev1";
    ```

3. run lint to check

    ```bash
    make lint
    ```

## available commands

```bash
make lint       # lint proto files with buf
```

## how other repos use this

the `grpc-template` repo generates Go code from this repo using:

generate all services:

```bash
buf generate https://github.com/kitti12911/proto-template.git
```

generate a specific service only (use `--path` to target a service directory):

```bash
buf generate https://github.com/kitti12911/proto-template.git --path example/v1
```

multiple services:

```bash
buf generate https://github.com/kitti12911/proto-template.git --path example/v1 --path other/v1
```

generated code goes to `gen/grpc/` in the consuming repo. see `grpc-template` for the full setup.

## buf config

`buf.yaml` uses:

- **lint:** STANDARD rules
- **breaking:** FILE-level breaking change detection
