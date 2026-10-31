# Multimodal Product-Cost Capture

**Focus:** LLM integration, multimodal input, and structured business data.  
**Status:** Proof of concept.  
**Stack:** n8n Cloud, Telegram Bot API, OpenAI Whisper, GPT-4o with vision, Google Sheets.

## Problem

A wholesale business lacked structured landed-cost data: the product cost after freight and related charges. The information would arrive gradually through the owner's existing communication habits, including voice notes, photos, and typed messages. A capture process needed to accept those formats rather than require a new manual-entry routine.

## My contribution

I built a Telegram bot and n8n workflow that converted those inputs into SKU-level records and returned a confirmation of what was saved.

```text
Voice note -> Whisper transcription --------\
Photo -> GPT-4o vision ---------------------+-> shared parser -> Google Sheets
Text -> GPT-4o field extraction ------------/                       |
                                               Telegram confirmation
```

The flow above is illustrative, not a deployable workflow export.

The structured record included timestamp, SKU, product name, landed cost, supplier, input type, and original input. Keeping the original input beside extracted fields supported later inspection. The confirmation reply displayed the extracted values for the sender to check; it was not a mandatory approval step before saving.

## Engineering decisions and debugging

Three mutually exclusive input paths fed one shared parser. A Merge node had blocked execution by waiting for branches that could not all emit for the same message; connecting the routes directly to the parser removed that dependency.

I also corrected missing reply context after the Sheets node replaced upstream data, fixed binary-file submission for transcription and image MIME handling, and used a known-supplier list to correct transcription errors in supplier names.

## Result and limits

The proof of concept implemented multimodal capture, structured storage, and confirmation replies. It created an input foundation for later analysis, rather than implementing margin optimization or upselling itself.

No deployment across the customer's full catalog, sustained production usage, extraction-accuracy benchmark, or financial uplift is claimed. Client scale, supplier names, product records, voice recordings, and photos are omitted.

[Back to portfolio](../README.md)
