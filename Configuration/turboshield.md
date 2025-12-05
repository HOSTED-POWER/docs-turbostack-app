---
order: 220
icon: umbrella
---
# TurboShield

TurboShield is our tool to battle high load caused by request floods. It does two things:
  - Rate limit for bot _User Agents_ (Nginx).
  - Temporarily block IP addresses that are making too many requests (Nginx & Apache).

## Configuration
To enable our configuration, add the following line above the **system_users**:
```yaml
turboshield:
  enabled: true
  level: medium # if not specified, medium is used
```
### Levels
Depending on your website, and the average amount of requests a visitor makes, you can choose the level. We recommend to look at your server logs to estimate the amount of requests a visitor makes, to avoid accidentally blocking legitimate visitors.

- **low**: 10 requests per second
- **medium**: 5 requests per second
- **high**: 1 request per second

### Allow and limit bots
You can allow or limit bots by adding them to the **allow_bots** or **limit_bots** list. All bots in the **allow_bots** list will be exempt from rate limiting, and all bots in the **limit_bots** list will be rate limited, according to the level parameter in the YAML, described above.
```yaml
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
This comes in handy when bots don't listen to the crawl-delay in robots.txt.