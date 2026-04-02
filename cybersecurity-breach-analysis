# Task 01 — Cybersecurity Breach Analysis

## Context

Daikibo Industrials deployed a real-time operational dashboard to monitor assembly line machine statuses across four factories:

- `meiyo`
- `seiko`
- `shenzhen`
- `berlin`

A major news publication leaked sensitive information about an assembly line failure, and Macora suspected the dashboard had been breached.

---

## Part 1 — Could a direct internet attack have caused the breach?

**Answer: No.**

The dashboard lives on Daikibo's **intranet**, not the public internet. Every IP address in the logs is from Daikibo's **internal network** and is **static** — meaning an attacker without access to Daikibo's VPN could not reach the dashboard at all. There is no externally exposed attack surface for someone on the internet to exploit directly. The breach, if it occurred, had to have originated from **inside the network** — either a compromised internal device or a user account that was hijacked.

---

## Part 2 — Log Analysis: Identifying Suspicious Activity

### Log file overview

The `web_requests.log` file contains blocks of HTTP requests grouped by internal IP address. Each block represents one or more browsing sessions from that device. Normal user behaviour follows this pattern:

```
Login → Load dashboard assets (HTML, CSS, JS) → API requests for machine statuses (on manual refresh)
```

The dashboard has **no automatic polling** — users must manually refresh to fetch new data.

---

### Suspicious IP: `192.168.0.101`

#### What was observed

This IP made API requests to all four factory endpoints **every hour, on the exact second (`:48`)**, continuously — starting from `00:00:48` on `2021-06-26`.

```
2021-06-26T00:00:48.000Z GET "/api/factory/machine/status?factory=meiyo&machine=*"
2021-06-26T00:00:48.000Z GET "/api/factory/machine/status?factory=seiko&machine=*"
2021-06-26T00:00:48.000Z GET "/api/factory/machine/status?factory=shenzhen&machine=*"
2021-06-26T00:00:48.000Z GET "/api/factory/machine/status?factory=berlin&machine=*"
-- repeats every hour --
```

This pattern repeated **every hour** from `00:00:48` through `16:00:48` (17 rounds = 68 requests), all returning `401 UNAUTHORIZED` — because the user was **not yet logged in**.

At `16:04`, the user finally logged in manually:

```
2021-06-26T16:04:00.000Z GET "/"                → 401 (not logged in)
2021-06-26T16:04:01.000Z GET "/login"            → 200
2021-06-26T16:04:54.000Z POST "/login"           → 200 (login successful)
2021-06-26T16:04:54.000Z GET "/" {userId: ...}   → 200
```

After login, the **exact same hourly pattern resumed** — now returning `200 SUCCESS`:

```
2021-06-26T17:00:48.000Z GET "/api/.../meiyo"    → 200
2021-06-26T17:00:48.000Z GET "/api/.../seiko"    → 200
2021-06-26T17:00:48.000Z GET "/api/.../shenzhen" → 200
2021-06-26T17:00:48.000Z GET "/api/.../berlin"   → 200
-- continues hourly --
```

---

#### Why this is suspicious

| Red flag | Explanation |
|---|---|
| **Exact hourly interval** | Requests hit at `XX:00:48` every single hour without deviation. No human refreshes a page this precisely — this is a script or automated tool. |
| **No login before API calls** | The script started polling the API before any login had occurred, suggesting it was running independently from any browser session. |
| **Querying all factories simultaneously** | Each polling round queries all four factories at the exact same second (`machine=*`), suggesting a script designed to scrape the full picture of machine statuses. |
| **No dashboard asset requests after login** | A real browser session would load CSS, JS, and images. After the login at `16:04`, only API endpoints were hit — no page assets — confirming the subsequent requests were scripted, not from a browser. |
| **Continued across the full day** | The activity ran from midnight to at least `21:00` — well beyond any plausible manual usage pattern. |

---

### Identified user

The `authorizedUserId` attached to all requests from this IP:

```
mdB7yD2dp1BFZPontHBQ1Z
```

This user account is associated with the automated scraping activity and is the primary suspect in the data breach.

---

## Summary

| Item | Finding |
|---|---|
| Direct internet attack possible? | **No** — dashboard is intranet-only |
| Suspicious IP | `192.168.0.101` |
| Suspicious user ID | `mdB7yD2dp1BFZPontHBQ1Z` |
| Attack method | Automated script polling all factory machine statuses every hour |
| Data exposed | Machine statuses for all four factories (meiyo, seiko, shenzhen, berlin) |
| Requests before login | 68 (all `401`) |
| Requests after login | At least 20 (all `200 SUCCESS`) |

---

## Skills demonstrated

- Reading and interpreting HTTP web request logs
- Identifying automated/scripted behaviour vs human browsing patterns
- Understanding authentication flows and session behaviour
- Explaining network security concepts (intranet vs internet, VPN access control)
- Communicating findings clearly to a non-technical client
