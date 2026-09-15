# artEst — Lot Estimate

A single-page tool that prices a photograph: upload an image of the print and
the artist's bio, and it returns a rough market-value estimate styled as an
auction-house lot ticket (price range, market tier, confidence, the factors
that drove the number, comparable context, and a caveat).

`index.html` is the Claude Artifact source for this tool. It calls the
`sample` runtime capability (`window.claude.use("sample")`) to have Claude
read the uploaded image and bio and return a structured estimate — that call
only works when the page is opened inside a Claude Artifact viewer, so
opening this file directly in a browser will render the UI but the
"Appraise this print" button won't be able to reach Claude.

Live version: https://claude.ai/artifact/R5cWoUcG9C1Bq5sGXHmPaN
