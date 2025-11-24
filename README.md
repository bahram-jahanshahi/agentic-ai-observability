# Agentic AI Observability

## Introduction

This thesis investigates how agent-based systems can perform root cause localization using observability data (logs, metrics, traces) following the OpenTelemetry standard. Using Google’s ADK and the OpenTelemetry demo application, you will design an agent that collects and reasons over telemetry data to propose likely root causes for incidents.

The research focus is on comparing different approaches to fusing multi-modal telemetry and exploiting service-dependency graphs, and on benchmarking an agentic approach against simpler correlation-based or rule-based baselines. You will run controlled fault-injection experiments, measure root cause ranking accuracy, and analyze robustness under noisy or incomplete telemetry.

This project is the most interesting in my opinion, but also requires the most work, since it also requires having built a root cause process that the agent can fetch information from. It also requires that you set up data storage/structure to handle all the telemetry data. Our team can support you in building parts of the project that you don’t think you’ll have time for.

## Observability with OpenTelemetry in Distributed Systems



### Overview
OpenTelemetry is an open-source observability framework that provides a standardized way to collect, process, and export telemetry data (traces, metrics, and logs) from distributed systems. It enables developers to gain insights into the performance and behavior of their applications, making it easier to diagnose issues and optimize performance.

### Architecture Components

### Logging Mechanism
1. When a request comes to system, a unique trace ID is generated to track the request across all services.
2. Each service (or span) involved in processing the request logs relevant information, including the trace ID, span_id, parent_id, status (OK, ERROR), timestamps, attributes and events.
3. Each service exports the collected telemetry data to a centralized database (preferably asynchronously) for storage and analysis.



### OpenTelemetry Database
Database should be able to store large volumes of telemetry data with high availability and scalability.

Database should support efficient querying and analysis of telemetry data.

## Discover root cause of an Incident (by Human)
1. Use trace IDs to follow the request path across services.
2. Query the database for logs, metrics, and traces associated with the trace ID.
3. Draw a service dependency graph to visualize interactions between services (with parent_id).
4. Analyze the collected data to identify root causes of issues and errors.

## Discover root cause of an Incident (by AI Agent)
1. Use trace IDs to follow the request path across services.
2. Create a prompt with the trace ID and relevant context.
A simple prompt template could be:
   ```shell
    Agent Role: You are an AI agent specialized in analyzing distributed system telemetry data.
    Task: Investigate the root cause of an incident using the provided trace ID.
    Trace ID: <trace_id>
    Logs: <logs associated with trace_id>
    Metrics: <metrics associated with trace_id>
    Traces: <traces associated with trace_id>
    Service Dependency Graph: <service dependency graph> 
    Output Format: Provide a detailed analysis of the root cause, affected services, and recommended actions to resolve the incident.
   ```
3. Send the prompt to the AI agent for analysis.
4. Agentic AI send the prompt to LLM for processing.
5. LLM processes the prompt and generates a response with the root cause analysis.
6. The AI agent reviews the LLM response and formats it for human consumption.
7. The AI agent presents the analysis to human operators for further action.

