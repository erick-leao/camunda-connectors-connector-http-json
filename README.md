# Camunda HTTP JSON Connector

Find the user documentation in our [Camunda Platform 8 Docs](https://docs.camunda.io/docs/components/integration-framework/connectors/out-of-the-box-connectors/rest/).

## Build

```bash
mvn clean package
```

## API

### Input

```json
{
  "method": "post",
  "url": "https://httpbin.org/anything",
  "queryParameters": {
    "q": "test",
    "priority": 12
  },
  "headers": {
    "User-Agent": "http-connector-demo"
  },
  "body": {
    "customer": {
      "id": 1231231,
      "name": "Jane Doe",
      "email": "jane.doe@exampe.com"
    }
  }
}
```

### Output

The response will contain the status code, the headers and the body of the response of the HTTP service.

```json
{
  "body": {
    "args": {
      "priority": "12",
      "q": "test"
    },
    "data": "{\"customer\":{\"id\":1231231.0,\"name\":\"Jane Doe\",\"email\":\"jane.doe@exampe.com\"}}",
    "files": {},
    "form": {},
    "headers": {
      "Accept-Encoding": "gzip",
      "Content-Length": "77",
      "Content-Type": "application/json",
      "Host": "httpbin.org",
      "User-Agent": "http-connector-demo Google-HTTP-Java-Client/1.41.4 (gzip)",
      "X-Amzn-Trace-Id": "Root=1-623105a8-35f88bac0c7f1dcf0d2c8aa2"
    },
    "json": {
      "customer": {
        "email": "jane.doe@exampe.com",
        "id": 1231231.0,
        "name": "Jane Doe"
      }
    },
    "method": "POST",
    "origin": "79.202.43.240",
    "url": "https://httpbin.org/anything?q=test&priority=12"
  },
  "headers": {
    "access-control-allow-credentials": "true",
    "access-control-allow-origin": "*",
    "connection": "keep-alive",
    "content-length": 733,
    "content-type": "application/json",
    "date": "Tue, 15 Mar 2022 21:31:20 GMT",
    "server": "gunicorn/19.9.0"
  },
  "status": 200
}
```

### Input (Basic Auth)

```json
{
  "method": "get",
  "url": "https://httpbin.org/basic-auth/user/password",
  "authentication": {
    "type": "basic",
    "username": "secrets.USERNAME",
    "password": "secrets.PASSWORD"
  }
}
```

### Output (Bearer Token)

```json
{
  "method": "get",
  "url": "https://httpbin.org/bearer",
  "authentication": {
    "type": "bearer",
    "token": "secrets.TOKEN"
  }
}
```

## Test locally

Run unit tests

```bash
mvn clean verify
```

### Test as local Job Worker

Use the [Camunda Job Worker Connector Run-Time](https://github.com/camunda/connector-framework/tree/main/runtime-job-worker) to run your function as a local Job Worker.

### :lock: Test as local Google Cloud Function

> **Warning**
> This is Camunda-internal only. The Maven profile `cloud-function` accesses an internal artifact.

Build as Google Cloud Function

```bash
mvn function:run -Pcloud-function
```

See also the [:lock:Camunda Cloud Connector Run-Time](https://github.com/camunda/connector-runtime-cloud) on how your function
is run as a Google Cloud Function.

## Element Template

The element templates can be found in
the [element-templates/http-json-connector.json](element-templates/http-json-connector.json) file.

## Build a release

Checkout the repo and branch to build the release from. Run the release script with the version to release and the next
development version. The release script requires git and maven to be setup correctly, and that the user has push rights
to the repository.

The release artifacts are deployed to Google Cloud Function by a GitHub workflow.

```bash
./release.sh 0.3.0 0.4.0
```
