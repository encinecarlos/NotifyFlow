# Technical Challenge: NotifyFlow

Build a multi-channel notification orchestration system composed of:

1. **A REST API** hosted on Azure App Service.
2. **An asynchronous processing service** hosted on Azure Functions.
3. **Centralized configuration** managed via Azure App Configuration.

## Required Features

### REST API (App Service)
- Allow the registration of notification channels (e.g., e-mail, SMS, webhook), including the necessary credentials and metadata.
- Allow the creation, listing, update, and removal of message templates with support for placeholders (e.g., `{{name}}`).
- Allow the definition of event-based dispatch rules, where each rule associates an event type with one or more channels and templates.
- Receive generic events via POST (`/events`) containing:
  - `type`: string identifying the event type.
  - `data`: JSON object with the contextual data of the event.
- Validate and enqueue events for asynchronous processing.

### Azure Function
- Consume events enqueued by the API.
- Evaluate active rules at the time of processing.
- For each applicable rule, render the corresponding template with the event data and send the notification through the configured channel.
- Record the result of each delivery attempt (success or failure) with a timestamp and relevant details.
- Be resilient to transient failures in external channels (e.g., retry with exponential backoff).

### Azure App Configuration
- Store all channel configurations (e.g., SMTP host, API keys, webhook URLs).
- Store dispatch rules in a way that allows updates without requiring a new deployment.
- Use feature flags to enable/disable specific channels or features.
- The application must read configurations directly from App Configuration, with support for automatic refresh.

## Technical Requirements

- The code must be written in C# (.NET 8).
- Must include unit and integration tests with a minimum coverage of 70% across the domain and application layers.
- The solution must be deployable to Azure using infrastructure scripts (e.g., ARM, Bicep, or Terraform) or clear deployment instructions.
- The API must expose a health check endpoint (`/health`) that returns the status of critical components.
- All relevant logs must be structured and compatible with Application Insights.
- The project must include a README.md with clear instructions for setup, local execution, and deployment.

## Evaluation Criteria

- Clarity and organization of the source code.
- Separation of concerns and maintainability.
- Quality and coverage of tests.
- Effective use of the specified Azure services.
- Error handling and system resilience.
- Performance and scalability of the solution.
- Documentation and ease of onboarding.
