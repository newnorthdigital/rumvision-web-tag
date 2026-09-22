# RUMvision GTM Web Tag Template

A Google Tag Manager web tag template for [RUMvision](https://www.rumvision.com) Real User Monitoring. It loads the RUMvision snippet to capture Core Web Vitals and other RUM metrics, follows GTM Consent Mode and can send custom dimensions from GTM variables.

## Features

- **Drop-in snippet loader**: enter your RUMvision property ID and the tag sets up the `rumv()` queue and loads the script from the RUMvision CDN
- **Consent Mode built in**: by default RUMvision only loads once `analytics_storage` is granted. Alternatively it loads right away in cookieless mode and switches storage on when consent is given
- **Consent withdrawal**: when `analytics_storage` is withdrawn, the tag switches RUMvision storage and device information off, which makes RUMvision delete the data it stored (needs the JavaScript API, see below)
- **Custom dimensions**: send values such as page type, release or A/B variant with `rumv('set')`, straight from GTM variables
- **Hostname override**: load the script for a different hostname (handy when monitoring a subdomain under the apex property)
- **Debug mode**: console logging in GTM Preview

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

1. Create a new tag using the **RUMvision Loader** template
2. Enter your **Property ID** (e.g. `RUM-OD60Q204MQ`). You find it in the RUMvision dashboard under **Manage → Settings → Snippet**
3. Pick a **Consent behaviour** (default: *Wait for analytics_storage consent*)
4. Set the trigger to **Initialization - All Pages**

The tag then loads `https://d5yoctgpv4cpx.cloudfront.net/<your-id>/v3-<hostname>.js` as soon as the chosen consent behaviour allows it.

If your site has a Content Security Policy, allow `script-src d5yoctgpv4cpx.cloudfront.net` and `connect-src p2iqhncxyh.execute-api.eu-central-1.amazonaws.com`.

### Requirement for cookieless mode and custom dimensions

The *cookieless until consent* and *never allow storage* options and custom dimensions work through the RUMvision JavaScript API. Switch on **Enable rumv() get/set API** under **Developer settings** for your domain in RUMvision. Without it, the RUMvision script ignores these settings, so it uses browser storage regardless of consent and drops the custom dimensions. The default *wait for consent* behaviour does not need the API to hold RUMvision back until consent. Switching storage off again after a visitor withdraws consent does need it, in every mode.

## Template fields

| Field | Required | Description |
|-------|----------|-------------|
| Property ID | Yes | RUMvision property ID, format `RUM-XXXXXXXXXX` |
| Consent behaviour | Yes | See below. Default: *Wait for analytics_storage consent* |
| Custom dimensions | No | Table of dimension name and value, sent with `rumv('set')`. The name must match a custom dimension in RUMvision. Empty values are skipped |
| Hostname override | No | Load the script for this hostname instead of the current page's hostname |
| Disable automatic data submission | No | Sets RUMvision's `auto: false` config |
| Log debug messages to console | No | Console logging in Preview mode. For RUMvision's own debug output, add `?rumv=debug` to the page URL |

### Consent behaviours

- **Wait for analytics_storage consent** (recommended): RUMvision loads only once `analytics_storage` is granted, either on page load or later when the visitor accepts. Works on every RUMvision setup.
- **Load immediately, cookieless until analytics_storage consent**: RUMvision loads right away with browser storage and device information off, and switches them on when `analytics_storage` is granted. You get Core Web Vitals from every visitor, including those who never accept. Needs the JavaScript API (see above).
- **Load immediately, always allow storage**: use only if a separate consent layer already stops the tag from firing without consent.
- **Load immediately, never allow storage**: RUMvision always runs without browser storage and device information. Needs the JavaScript API.

In both consent-following modes, withdrawing `analytics_storage` later switches storage and device information off again (needs the JavaScript API).

In wait mode the tag does not complete until consent is granted, so tags sequenced to fire after it wait as well.

In the cookieless mode, custom dimensions are sent before consent. Never map personal data such as user IDs or email addresses to them.

Sites without Consent Mode set up are treated as granted, so the tag loads as usual.

## Permissions

- **Inject scripts**: loads from `d5yoctgpv4cpx.cloudfront.net` (the RUMvision CDN)
- **Access global variables**: read/write/execute `window.rumv` and read/write `window.rumv.q`
- **Access consent state**: reads `analytics_storage`
- **Get URL**: reads the page hostname for the script URL
- **Logging**: console logging in debug/preview mode only

## Resources

- [RUMvision website](https://www.rumvision.com)
- [Install the snippet](https://www.rumvision.com/help-center/monitoring/getting-started/install-the-snippet/)
- [JS API reference](https://www.rumvision.com/help-center/apis/js/)

## Author

Created and maintained by [Freek Kampen](https://freekkampen.com) at [New North Digital](https://newnorth.digital?utm_source=github&utm_medium=gtm-template&utm_campaign=rumvision-web-tag).

## License

Apache 2.0, see [LICENSE](LICENSE).
