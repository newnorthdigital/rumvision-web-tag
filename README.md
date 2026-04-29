# RUMvision — GTM Web Tag Template

A Google Tag Manager web tag template for [RUMvision](https://www.rumvision.com) Real User Monitoring. Loads the RUMvision snippet to capture Core Web Vitals and other RUM metrics, with first-class support for GTM Consent Mode v2.

## Features

- **Drop-in snippet loader** — Provide your RUMvision property ID and the tag handles the queue setup and CDN script injection
- **Consent Mode v2 aware** — Maps RUMvision's `consent_storage` setting to GTM's `analytics_storage` so cookies are only set after the visitor accepts analytics
- **Always-on / cookieless modes** — Override the default and either always allow storage or run RUMvision in cookieless mode
- **Hostname override** — Force the script URL to a different hostname (handy when monitoring a subdomain under the apex property)
- **Manual submit mode** — Disable automatic submission so you can call `rumv('send')` from a separate tag
- **Debug mode** — Console logging in GTM Preview/Debug

## Installation

### Gallery (recommended)

1. In GTM, click **New Tag → Tag Configuration → Discover more tag types in the Community Template Gallery**
2. Search for **RUMvision** (publisher: New North Digital)
3. Click **Add to workspace**

### Manual

1. Download `template.tpl` from this repo
2. In GTM, go to **Templates → New** in the *Tag Templates* section
3. Click the overflow menu → **Import** and select the `.tpl` file

## Setup

1. Create a new tag using the **RUMvision** template
2. Enter your **Property ID** (e.g. `RUM-OD60Q204MQ`) — found in the RUMvision dashboard under **Manage → Settings → Snippet**
3. Pick a **Storage consent** strategy (default: *Follow GTM Consent Mode*)
4. Set the trigger to **Initialization - All Pages** (or **Consent Initialization** if you want it to run before consent is established)

That's it. The tag will:
- Set up the `rumv()` queue
- Default `consent_storage` to `0`
- Listen for `analytics_storage` and switch to `1` once granted
- Inject `https://d5yoctgpv4cpx.cloudfront.net/<your-id>/v3-<hostname>.js`

## Template Fields

| Field | Required | Description |
|-------|----------|-------------|
| Property ID | Yes | RUMvision property ID, format `RUM-XXXXXXXXXX` |
| Storage consent | Yes | `Follow GTM Consent Mode` (default), `Always allow`, or `Never allow` |
| Hostname override | No | Override the hostname used in the script URL |
| Disable automatic data submission | No | Sets RUMvision's `auto: false` config |
| Log debug messages to console | No | Console logging in Preview mode |

### Consent strategies in detail

- **Follow GTM Consent Mode** — Storage starts denied. The tag listens for `analytics_storage` and grants storage when it flips to granted. Recommended when GTM's consent state is the source of truth.
- **Always allow storage** — Sets `consent_storage` to `1` immediately. Use only if you have an external consent layer that already prevents the tag from firing without consent.
- **Never allow storage** — Keeps `consent_storage` at `0`. RUMvision still collects RUM metrics but in a cookieless mode.

## Permissions

- **Inject Scripts** — Loads from `d5yoctgpv4cpx.cloudfront.net` (the RUMvision CDN)
- **Access Global Variables** — Read/write/execute `window.rumv` and `window.rumv.q`
- **Access Consent State** — Reads `analytics_storage`
- **Get URL** — Reads the page hostname for the script URL
- **Logging** — Console logging in debug/preview mode only

## Resources

- [RUMvision website](https://www.rumvision.com)
- [Install the snippet](https://www.rumvision.com/help-center/monitoring/getting-started/install-the-snippet/)
- [JS API reference](https://www.rumvision.com/help-center/apis/js/)

## Author

Created by [New North Digital](https://newnorth.digital?utm_source=github&utm_medium=gtm-template&utm_campaign=rumvision-web-tag).

## License

Apache 2.0 — see [LICENSE](LICENSE).
