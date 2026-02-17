---
icon: shield-x
order: 220
---

# TurboShield

**TurboShield** helps protect your website from traffic spikes and
request floods that can slow down or take your site offline. It
automatically detects abusive traffic and limits or blocks it before it
affects real visitors.

TurboShield is also especially useful for handling **aggressive scraping
bots**. Instead of blocking them outright - which may not always be
desirable - TurboShield can **rate limit bots** so they consume fewer
resources while still being able to access your site at a controlled
pace. This keeps your server responsive for real users while avoiding
unnecessary bot bans.

TurboShield works by:

-   Rate limiting requests for bot and automated traffic (via nginx)
-   Temporarily blocking IP addresses generating excessive traffic (via
    nginx)
-   Automatically banning repeat offenders using **Fail2ban**
-   Blocking outdated or suspicious user agents at the highest protection level

The result: your website stays fast and available even during aggressive scraping or an overload of malicious traffic.

## Configuration

To enable TurboShield, add the following configuration **above
`system_users`**:

``` yaml
turboshield:
  enabled: true
  level: medium # defaults to medium if omitted
```

Once enabled, protection is applied automatically.

## Protection Levels

TurboShield offers multiple protection levels, each with their own rate limits.

If you're unsure, check your server logs first to avoid blocking
legitimate users.

| Level                 | Requests per second per IP | Recommended use                      |
|-----------------------|----------------------------|--------------------------------------|
| **low**               | 6 req/sec                  | Normal websites with moderate traffic |
| **medium** (default)  | 2 req/sec                  | Most production sites                |
| **high**              | 1 req/sec                  | Aggressively scraped sites           |
| **under attack mode** | 1 req/sec                  | Sites actively under attack          |

### Under attack mode

Aside from the usual levels, you can also activate the **under attack mode**, which includes the rate limit of level **high**, with some extra functionality:

-   Blocks **outdated and suspicious user agents** 
-   Reduces abuse from legacy or automated clients often used in
    scraping and attack tools

This adds an extra layer of protection when your site is under heavy
automated traffic.

!!! attention
This mode might block some legitimate traffic if the client uses an outdated browser! However, the impact should be limited as modern browsers usually update automatically.
!!!

## Fail2ban Integration

TurboShield includes **Fail2ban** integration.

Fail2ban monitors logs for abusive request patterns and automatically:

-   Detects IPs repeatedly exceeding limits
-   Temporarily bans them at firewall level
-   Extends bans for repeat offenders

This prevents aggressive bots from continuously retrying after rate
limiting.

## Allowing or Limiting Bots

Some bots are useful (search engines), while others can overload your
server. TurboShield allows fine-grained control.

You can:

-   **Allow bots** to bypass rate limits
-   **Limit bots** to enforce rate limiting

### Example configuration

``` yaml
turboshield:
  enabled: true
  level: high
  allow_bots:
    - bingbot
    - Mozilla/5.0
  limit_bots:
    - GoogleBot
    - bytespider
```

### Behaviour

-   Bots in `allow_bots` bypass rate limiting.
-   Bots in `limit_bots` are rate limited according to the configured
    level.
-   All other traffic follows the default protection rules.

!!! info
This is especially helpful when bots ignore `crawl-delay` rules in
`robots.txt`!
!!!


## Recommended Setup

For most websites:

-   Start with **medium** (default if unspecified)
-   Monitor logs and performance
-   Increase to **high** if your site still experiences disruptive scraping or traffic
    floods

If legitimate users are blocked, lower the level or allow trusted bots.
