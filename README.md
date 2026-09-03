# Flask Circuit Breaker Demo

A small Flask service that demonstrates the circuit breaker pattern with `pybreaker`. The service wraps an intentionally unreliable operation, opens the circuit after repeated failures, and exposes the current breaker state.

## Behavior

The simulated operation fails randomly. The breaker is configured to open after three consecutive failures and attempt recovery after ten seconds.

| State | Behavior |
|---|---|
| Closed | Calls are allowed and failures are counted |
| Open | Calls are rejected with HTTP 503 |
| Half-open | A trial call determines whether normal traffic can resume |

## Setup

```bash
git clone https://github.com/zanax1990/Circutbreaker.git
cd Circutbreaker
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

On Windows, activate the environment with `.venv\Scripts\activate`.

The development server listens on `http://localhost:5001`.

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| GET | `/` | Basic health response |
| GET | `/unstable` | Run the simulated unreliable operation |
| GET | `/circuitbreaker` | Return the breaker state and failure count |

Example:

```bash
curl http://localhost:5001/unstable
curl http://localhost:5001/circuitbreaker
```

## Limitations

Breaker state is stored in memory and is not shared across processes or replicas. Failures are random rather than produced by a real downstream service. The Flask debug server is intended for local demonstration only. Automated tests and production deployment configuration are not included.
