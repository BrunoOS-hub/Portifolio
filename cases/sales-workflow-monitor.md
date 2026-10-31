# Sales Workflow Monitoring

**Focus:** Integration discovery, stateful automation, and operational notifications.  
**Status:** Used daily by a small sales team, as reported from the client engagement.  
**Stack:** n8n, Upstash Redis, Telegram Bot API, Chrome/Edge extension.

## Problem

Salespeople repeatedly opened a business platform to check document approvals, invoicing readiness, and process-stage changes. Updates were discovered only when somebody remembered to look. The required workflow had no documented integration endpoints or webhooks.

## My contribution

I inspected the application's browser network requests, identified the retrieval path used by the existing interface, and built a browser-assisted integration. A Chrome/Edge extension connected the authenticated session to the n8n workflow; the implementation details of that authentication exchange are not published here.

- Scheduled process checks every 15 minutes for the relevant salespeople.
- Compared current process state with the previously stored state in Redis.
- Sent Telegram notifications about detected changes to the responsible salesperson.
- Added separately scheduled session-expiration checks and renewal notifications.

## Technical flow

```text
Existing authenticated session -> browser-assisted connection
Scheduled n8n check -> current process state -> compare with Redis
Detected change -> Telegram notification to the responsible salesperson
Session-expiration check -> renewal notification
```

## Engineering decision: conventional automation, not an AI agent

The core requirement was to detect a known state change and notify the right person. That did not require an LLM. Scheduled polling and explicit state comparison addressed the problem without adding model uncertainty to routine operational alerts.

## Result and limits

The system entered daily use by a small sales team. It moved routine status discovery toward scheduled checks and targeted notifications instead of relying entirely on repeated manual platform visits. Team size is generalized for confidentiality.

The 15-minute schedule is a polling interval, not an instant-delivery guarantee. Authentication renewal and upstream application behavior remain dependencies. No measured time-savings percentage, permanent zero-cost claim, or broad availability SLA is asserted.

No live session values, local-storage key names, endpoint URLs, or authentication-handling code are included. This case describes the integration approach, not an invitation to access a third-party system or reproduce it without authorization.

[Back to portfolio](../README.md)
