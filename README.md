# Datadog Reinstrumentation Example

This repository demonstrates how to migrate from Datadog's APM and metrics collection to an OpenTelemetry-based observability stack. It builds upon Datadog's `apm-tutorial-golang` to showcase a vendor-agnostic approach for tracing and metrics collection.

For changes made, please refer to the first commit to this repository. Changes include:

- Setting the OpenTelemetry Collector as `DD_AGENT_HOST`
- Configuring the OpenTelemetry Collector using the Datadog Receiver to collect from the default ports for Datadog APM and DogStatsD
- Adjusting the Agent Address in the application code to make sure the right address where the agent is located is set

## Getting Started

To run this example:

1. Clone the repository
2. Navigate to the directory containing the docker-compose.yml file:

    ```bash
    cd docker # from the project root
    ```

3. Run docker-compose:

    ```bash
    docker-compose -f all-docker-compose.yaml build
    ```

    ```bash
    docker-compose -f all-docker-compose.yaml up -d
    ```

You can then access:

- Prometheus UI at http://localhost:9090
- Jaeger UI at http://localhost:16686

This setup allows you to verify that traces and runtime metrics are properly collected and received by the respective systems.

## Example Requests

Here are some sample curl commands to interact with the notes application:

```bash
# Get all notes
curl localhost:8080/notes
```

```bash
# Create a new note
curl -X POST 'localhost:8080/notes?desc=hello'
```

```bash
# Get a specific note
curl localhost:8080/notes/1
```

```bash
# Update a note
curl -X PUT 'localhost:8080/notes/1/desc=UpdatedNote'
```

```bash
# Delete a note
curl -X DELETE 'localhost:8080/notes/1'
```

```bash
# Create a note with a date
curl -X POST 'localhost:8080/notes?desc=hello_again&add_date=y'
```

## Key Features

- **OpenTelemetry Collector Integration:** Replaces the Datadog Agent with the OpenTelemetry Collector for data ingestion.
- **Prometheus Integration:** Collects and visualizes runtime metrics.
- **Jaeger Integration:** Provides distributed tracing capabilities.
- **Multi-Service Setup:** Includes two services (`notes` and `calendar`) to demonstrate distributed tracing.

Note: Prometheus and Jaeger are just means to an end in order to test the setup. Any endpoint that is supported by the OpenTelemetry Collector can be used.

## Architecture Overview

The example setup consists of the following components:

1. **Application Services:** `notes` and `calendar` services instrumented with Datadog's SDK.
2. **OpenTelemetry Collector:** Acts as a drop-in replacement for the Datadog Agent.
3. **Prometheus:** Collects and stores metrics data.
4. **Jaeger:** Collects and visualizes distributed traces.

## Key Changes

**OpenTelemetry Collector Configuration:**

- Set as `DD_AGENT_HOST` for application services.
- Configured to receive Datadog APM and DogStatsD data.

**Application Code Adjustments:**

- Updated agent address to point to the OpenTelemetry Collector.

**Docker Compose Setup:**

- Includes services for applications, OpenTelemetry Collector, Prometheus, and Jaeger.

## Example Configuration

The repository includes a docker-compose.yml file that sets up the entire stack, including:

- Application services (`notes` and `calendar`)
- OpenTelemetry Collector
- Prometheus
- Jaeger

The OpenTelemetry Collector is configured to:

- Receive Datadog APM data on port 8126
- Receive DogStatsD metrics on port 8125
- Export traces to Jaeger
- Export metrics to Prometheus

For detailed configuration, refer to the `otel-collector-config.yaml` file in the repository.

---

I extend my sincere gratitude to Datadog for their excellent `apm-tutorial-golang` repository, which served as the foundation for this project. Their open-source contribution under the Apache License has been instrumental in creating this example.
