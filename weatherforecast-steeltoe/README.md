# Steeltoe Sample Accelerator

A sample accelerator for Steeltoe.

This sample is based on the Weather Forecast RESTful API application made available from Microsoft.

The application also includes several Steeltoe features: management endpoints, dynamic logging, and distributed tracing.

The starting source for this sample was created using:
```
$ dotnet new steeltoe-webapi --logging-dynamic-logger --management-endpoints --distributed-tracing
```

## Running the app locally

To run the sample application:

```
$ dotnet run
```

For more details on Steeltoe endpoints, visit https://docs.steeltoe.io/.
