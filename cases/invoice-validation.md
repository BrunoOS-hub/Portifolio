# Serverless Invoice Validation

**Focus:** Document intelligence, cloud integration, and reliable external effects.  
**Status:** Validated in staging; commercial production activation not established.  
**Stack:** Python, AWS Lambda, Amazon Textract, Amazon S3, DynamoDB, CloudWatch, REST APIs.

## Problem

A commercial program needed to convert real invoice images into validated product quantities and sales records. Extracting readable text was not enough: product eligibility, quantity normalization, uncertain input, and repeated submissions all affected whether an external registration should occur.

## My contribution

I designed and implemented the document-processing workflow, integration logic, and validation controls, then exercised the flow with real invoices in a controlled staging environment.

- Used Textract and Python to extract document information, interpret line items, identify eligible products, and normalize quantities into standard units.
- Orchestrated processing with Lambda, persisted processing state in DynamoDB, and retained evidence in S3 with operational logging through CloudWatch.
- Integrated downstream APIs for registration across distinct participant profiles.
- Implemented duplicate detection, request idempotency, durable processing, controlled retries, and traceable external-effect records.

## Technical flow

```text
Invoice image -> Textract -> Python parsing and normalization -> validation
Validation -> eligible registration OR no-sale/review outcome
DynamoDB: processing state | S3: evidence | CloudWatch: operational logs
```

This is a simplified view; client-specific rules, payloads, infrastructure identifiers, and API addresses are intentionally excluded.

## Engineering decision: validation before external effects

OCR confidence is not business approval. Registration depends on validated business data, while uncertain inputs can remain unregistered for review. Request replay and exact-duplicate handling are separate from broader similarity judgments; a different photograph of the same document is not automatically guaranteed to be detected.

## Recorded result and limits

Selected real invoices completed the staging extraction, validation, and downstream registration flow. Replaying a previously accepted request did not create a second sales registration in the verified cases. Inconclusive documents could return a no-sale outcome rather than trigger an unsupported registration.

The result is a staging validation, not evidence of unrestricted production coverage, live commercial rollout, financial savings, or a universal document-accuracy rate. Duplicate handling protects data integrity; it is not presented as a general fraud-detection system.

[Back to portfolio](../README.md)
