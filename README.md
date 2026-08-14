[![Appwrite](https://img.shields.io/badge/Appwrite-F02E65?style=for-the-badge&logo=appwrite&logoColor=white)](https://appwrite.io/)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/docs/Web/JavaScript)
[![License: MIT](https://img.shields.io/badge/License-MIT-0F5132?style=for-the-badge)](./LICENSE)

An arrival planner for new international students starting at the **University of Limerick** in Autumn 2026 — checklist, calendar, customs rules, transport notes and contacts, in one page that works offline.

Check out the live site [here](https://ul-arrivals-2026.appwrite.network).

Built while planning one move from Delhi to Limerick. Most of what's here was hard to find, scattered across a dozen UL pages and emails, or simply wrong in the packing lists that circulate among students. It's shared in case it saves somebody else the digging.

## What's in it

| View | Contents |
|---|---|
| **Checklist** | ~33 items grouped by when they have to happen, adapting to your situation |
| **Calendar** | August–September 2026 with Week 0 shaded and hover detail on every dated cell |
| **Timeline** | The same events as a vertical run-up |
| **Getting here** | Why Dublin beats Shannon, one-stop routings, transit-visa rules, airport→campus coaches |
| **Packing** | Irish customs rules for food from India, do-not-pack lists, cash limits, medicines |
| **Reference** | Webinar recordings, faculty orientation days, village contacts, IRP requirements, finding accommodation |

Two toggles in the sidebar — **visa waiting/approved** and **housing sorted/looking** — add and remove items so the list matches where you actually are.

## Corrections it encodes

A few things that are easy to get wrong, and that this page gets right:

- **Baggage.** Google Flights reports weight-based allowances as piece counts and gets Gulf carriers wrong. Check the airline's own fare rules.
- **Faculty orientation.** Runs 1–4 September, one faculty per day, and is a *separate* schedule from the UL Global international programme. Reading only one means missing the other.
- **IRP registration.** Limerick residents register at Henry Street Garda Station, not Burgh Quay in Dublin.
- **Food from India.** Dairy and meat from non-EU countries cannot be imported at all. Ghee and milk sweets are the classic seizure — destroyed, not returned.
- **Arrival timing.** A 19:10 landing realistically means campus around 23:00–23:45 once immigration, bags and an hourly coach are accounted for.

Everything was checked against `ul.ie` and Irish government sources in August 2026. **Rules change** — re-verify anything time-sensitive, especially customs and immigration.

## Structure

The site lives in [sites/ul-arrivals](./sites/ul-arrivals) as a single self-contained `index.html` — no build step, no bundler, no dependencies. The repo root keeps the wrapper files, [appwrite.config.json](./appwrite.config.json) and convenience scripts.

```
sites/ul-arrivals/index.html   the whole thing
appwrite.config.json           Appwrite Sites deployment config
package.json                   dev + deploy scripts
```

## Development

```bash
bun run dev      # serves sites/ul-arrivals on :8080
```

Or just open `sites/ul-arrivals/index.html` in a browser. Everything except the optional sync works from `file://`.

## Deploying

```bash
bun run deploy   # appwrite push site
bun run domain   # repoint the stable domain at the new deployment
```

The domain step matters: Appwrite gives each deployment its own preview URL, and the stable domain is a `manual`-trigger proxy rule pinned to a deployment ID. Skip it and the URL keeps serving the previous build.

## Sync

Ticks save to `localStorage` by default, so the page is fully usable with no account. Signing in with an email and password syncs them across devices via Appwrite Auth and TablesDB.

Each user's list is one row keyed to their user ID, written with permissions scoped to that user alone — nobody can read anyone else's, including the maintainer. The table has row-level security on and only `create("users")` at table level.

To point this at your own Appwrite project, change the three constants near the top of the script block:

```js
const ENDPOINT='https://sgp.cloud.appwrite.io/v1';
const PROJECT='your-project-id';
const DB='main', TABLE='arrivals';
```

You'll need a `longtext` column named `state`, row security enabled, and your domain registered as a Web platform — without that last one the SDK is blocked by origin checks.

## Contributing

If you spot something out of date or wrong, open an issue or a PR. Dates, fees and immigration rules shift every intake, and a correction from someone who has just been through it is worth more than anything I can verify from a browser.

## License

MIT — see [LICENSE](./LICENSE).

## Acknowledgements

Facts drawn from [ul.ie](https://www.ul.ie), UL Global's offer-holder webinars, Ireland's [Citizens Information](https://www.citizensinformation.ie), and the UK Home Office transit guidance. None of it is official University of Limerick material.
