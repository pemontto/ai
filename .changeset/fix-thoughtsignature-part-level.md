---
'@tanstack/ai-gemini': patch
---

fix(ai-gemini): read/write thoughtSignature at Part level, not nested inside functionCall

Gemini 3.x models emit `thoughtSignature` as a sibling of `functionCall` on the Part object,
not nested inside it. The previous fix (#401) read from `functionCall.thoughtSignature` and
wrote it back nested, which the Gemini API rejects with 400 INVALID_ARGUMENT on the second turn.

- READ: check `part.thoughtSignature` first, fall back to `functionCall.thoughtSignature` for
  backwards compatibility with any models that may nest it.
- WRITE: emit `thoughtSignature` as a Part-level sibling of `functionCall`.
