# rkik-ntp-drift-check

GitHub Action to verify the time drift between two NTP servers using the [rkik](https://crates.io/crates/rkik) Rust crate.

This action is useful in CI/CD pipelines and distributed systems where accurate time synchronization is critical.  
It fails if the absolute drift between the two servers exceeds a specified threshold in milliseconds.

---

## Features

- Compares two NTP servers
- Extracts time drift (`difference_ms`) using rkik's JSON output
- Fails the job if drift exceeds a configurable threshold
- Runs in any Ubuntu GitHub Actions runner

---

## Usage

### Example Workflow

```yaml
name: Check NTP Drift

on: [push]

jobs:
  ntp-drift:
    runs-on: ubuntu-latest
    steps:
      - uses: your-github-username/rkik-ntp-drift-check@v1
        with:
          server_a: "0.fr.pool.ntp.org"
          server_b: "1.fr.pool.ntp.org"
          max_drift_ms: 1.0
```

---

## Inputs

| Name           | Description                                         | Required | Default |
|----------------|-----------------------------------------------------|----------|---------|
| `server_a`     | First NTP server to compare                         | Yes      | —       |
| `server_b`     | Second NTP server to compare                        | Yes      | —       |
| `max_drift_ms` | Maximum allowed drift in milliseconds (float)      | No       | `50`    |

---

## Behavior

This action runs `rkik --compare <server_a> <server_b> --format json`  
It parses the `.difference_ms` field from the JSON output and fails the job if it exceeds the `max_drift_ms` input.

---

## Requirements

- `jq` is used to parse the JSON output from `rkik`
- Internet access to resolve and reach NTP servers

---

## License

MIT
