# JakMa Engine

[![CI](https://github.com/Samielakkad/AI-LLM-Agents-JakMa-Engine/actions/workflows/ci.yml/badge.svg)](https://github.com/Samielakkad/AI-LLM-Agents-JakMa-Engine/actions/workflows/ci.yml)
[![Live site](https://img.shields.io/badge/live-jak.ma-176B87)](https://jak.ma)

Public engineering mirror of the retrieval, Darija classification and tool-calling code used by [jak.ma](https://jak.ma), a Moroccan home-services marketplace.

Production records, credentials and deployment history are deliberately excluded. The live service can also move ahead of this mirror.

## What is verifiable here

| Part | Code | Tests |
| --- | --- | --- |
| Darija and Arabizi trade/city classification | [`lib/text-classifier.js`](lib/text-classifier.js) | [`tests/text-classifier.test.js`](tests/text-classifier.test.js) |
| Bounded tool-calling and worker allow-list checks | [`lib/agent-loop.js`](lib/agent-loop.js), [`lib/tools.js`](lib/tools.js) | [`tests/agent-loop.test.js`](tests/agent-loop.test.js) |
| Candidate retrieval and citation verification | [`lib/grounded-retrieval.js`](lib/grounded-retrieval.js) | [`tests/grounded_retrieval.test.js`](tests/grounded_retrieval.test.js), [`tests/grounded_integration.test.js`](tests/grounded_integration.test.js) |
| Price-band rules and fallback behavior | [`lib/price-fairness.js`](lib/price-fairness.js) | [`tests/price-fairness.test.js`](tests/price-fairness.test.js) |

The full local suite currently contains 170 passing `node:test` cases. They use fixtures and mocks to check deterministic behavior; they do not establish production accuracy, traffic share, latency, cost or reliability.

## Run the checks

```bash
npm ci
npm test
```

No database or provider key is needed for the test suite.

## Run the server

```bash
npm ci
npm start
```

The complete server expects provider configuration and, for production-backed routes, MongoDB. Do not put credentials in the repository. The deterministic classification, retrieval and fairness tests are the easiest way to inspect the public code without production access.

## Request flow

```text
query
  -> follow-up/tool intent check
  -> Darija rule classifier or provider fallback
  -> allow-listed worker retrieval
  -> streamed response
  -> cited-worker and price checks
```

Implementation details and historical design notes are in [`ARCHITECTURE.md`](ARCHITECTURE.md) and [`docs/`](docs/). Numerical traffic, cost and latency figures in those notes are not reproduced by this public mirror unless a file explicitly includes the dataset and command used to calculate them.

## Not included

- Worker names, phone numbers, addresses or other production records
- Provider keys, database credentials or private environment files
- Private evaluation logs and service dashboards
- LoRA training data; the separately published adapter has its own model card and license

## License

All rights reserved. The repository is public for review and reference, not open-source reuse. See [`LICENSE`](LICENSE).
