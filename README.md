# Waypoint

A static website for planning long-haul flights. Visitors enter a route, their dates and how flexible they are, and pick the kind of layover they want. Waypoint then:

- opens price-sorted searches on **Skyscanner, Google Flights, Kayak and Momondo** with the flexibility already applied (± days, or a whole-month view);
- lists **date combinations to try**, flagging midweek departures;
- ranks **connecting airports** along the route for the chosen layover style (quick change, relaxed airside, see the city, or a multi-night stopover), with notes on comfort, getting into town, stopover programmes and transit visas;
- builds **multi-city searches** that include the stopover, plus separate one-way searches for each leg;
- produces a **shareable link** for every plan, and (optionally) a GitHub issue form so people can send you their trip requests.

It's one HTML file with no build step and no server, so it runs on GitHub Pages for free.

## Put it online with GitHub Pages

1. Create a new public repository on GitHub, for example `waypoint`.
2. Upload `index.html`, `README.md` and the `.github` folder (keep the folder structure).
3. In the repo, go to **Settings → Pages**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, and save.
4. After a minute your site is live at `https://YOUR-USERNAME.github.io/waypoint/`.

## Let people send you requests

Near the top of the `<script>` in `index.html`, set:

```js
repo: "YOUR-USERNAME/waypoint",
```

An **Ask for help planning** link appears in the header. It opens the issue form in `.github/ISSUE_TEMPLATE/flight-request.yml`, where people describe their trip. You reply in the issue with links (tip: fill in the planner yourself and paste the **Copy link to this plan** URL). Anyone sending a request needs a free GitHub account; if you'd rather accept requests from anyone, swap the link for a form service such as Google Forms, Tally or Formspree.

## Customise

- **Regional sites:** change `skyscannerDomain`, `kayakDomain` and `momondoDomain` in `CONFIG` (e.g. `www.skyscanner.co.nz`, `www.kayak.co.nz`) so prices show in your visitors' currency.
- **Airports:** add rows to `AIRPORTS` as `[code, city, country, latitude, longitude]`. Any three-letter code still works for fare searches even if it isn't in the list; the list only powers the map and layover suggestions.
- **Layover hubs:** edit `HUBS`. Each hub has four 0–10 scores (`s: [comfort, easy transfers, city access, stopover perks]`) plus notes. These are editorial judgements and visa rules change often, so review them now and then.
- **Ranking:** `STYLE` sets how much each score matters for each layover style, and `rankHubs` sets how much extra distance a detour may add (22% on very long routes, 35% otherwise).

## About live prices

GitHub Pages can only serve static files, and flight-price APIs need a secret key that can't be put in public JavaScript. That's why Waypoint hands off to the comparison sites, which always show current prices.

To show prices on the page itself, add a small serverless function (Cloudflare Workers, Netlify Functions or Vercel all have free tiers) that holds the API key and calls a flight data provider such as Duffel, Travelpayouts or Kiwi.com's partner API. The page would call your function instead of the provider directly. Check each provider's current terms, since access and pricing change.

## Disclaimer

Waypoint doesn't sell tickets or guarantee prices. Deep links follow each site's public URL format, which the sites can change without notice. Always confirm visa and transit rules for your passport before booking.
