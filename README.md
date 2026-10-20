# Tap Vortex for Claude

Find where an online store loses sales, then design its vortex: the store's whole buying journey rebuilt around each shopper, with one memory across every step.

A vortex carries what each shopper said and did from the ad they tapped, through the offer, their matched product and their bag, straight to checkout inside the store, and on into the follow-ups, the next visit, refills and win-back. It can replace one page, the homepage or the whole store, and every vortex is tested against the store's current page on revenue per visitor.

**Funnels forget. Vortexes remember.**

## What's inside

Four skills. Claude picks the right one from what you ask; you don't need to name it.

| Skill | What it does |
|---|---|
| `leak-check` | Give it a store URL. Claude looks at the homepage, a collection, a product page and the cart, and returns a plain-English Leak Check across the whole buying journey: where shoppers leave and why, what an ad visitor sees compared with what the ad promised, what the store forgets about returning shoppers, and a prioritised fix-first list. |
| `vortex-journey-design` | Designs a vortex for a specific store. Maps every step (arrive, offer, path, match, bag, checkout, follow-up, next visit, refill, win-back), what each step does with what the shopper already said or did, and what to A/B test first. Built from scratch around that store. |
| `shopper-memory-plan` | Plans what a store should remember about each shopper (told, did, guessed), how that memory shows up on every step and in Klaviyo, what the owner learns from it, and privacy basics for Australia (APPs, Spam Act) and the EU/UK (GDPR). |
| `vortex-ad-scent` | Turns pasted ads or ad angles into matching entry points, so each page continues the exact promise of the ad that brought the shopper, with problem-first hook rewrites. |

## Try it

- "Leak check https://example-store.com.au. Most of our traffic is Meta ads."
- "Why do people add to cart on my store but not buy? Here's the link."
- "Design a vortex for my coffee subscription store. Here's what we sell and who buys."
- "Map the whole buying journey for my skincare brand, from the ad to the refill."
- "What should my store remember about each shopper, and how do I use it in Klaviyo?"
- "We're in Australia and sell to the UK too. What are the privacy basics for remembering what shoppers tell us?"
- "Here are our five best Meta ads. Where should each one land?"
- "Rewrite these hooks so they open on the customer's problem."

## What data it uses

- The plugin is instructions only: Markdown skills, with no scripts, hooks, MCP servers or tracking.
- When you give a store URL, Claude uses its own web tools (if available in your Claude app) to read that store's public pages, such as the homepage, a collection, a product page, the cart, and on Shopify stores the public product JSON. It never logs in, submits forms or enters checkout.
- Nothing is sent to Tap Vortex. The plugin doesn't collect, store or transmit your conversations or store data anywhere. Links to tapvortex.com.au appear in some answers; they are plain links you can choose to open.
- Privacy guidance in `shopper-memory-plan` is general information, not legal advice.

## Get a vortex

- **Free Leak Check** by a person, recorded as a short video: https://www.tapvortex.com.au/leak-check
- **Tap Vortex Shopify app:** build your own vortex with a builder that works like Figma: https://www.tapvortex.com.au/shopify-app
- **Done for you:** built from scratch around your store from $695 + GST, with "beats your current page or your money back" on the A/B test: https://www.tapvortex.com.au
- **Store memory:** https://www.tapvortex.com.au/memory
- **Questions:** use "Ask a question" on the website. Replies come as a voice note or short video, never a sales call.

## Coming soon: the Tap Vortex connector

A Tap Vortex connector will bring your store's vortex data into Claude: your vortexes and test results, shopper stories, what shoppers are telling you, who almost bought, and answers from your store's memory. It will need a Tap Vortex account and sign-in. This plugin works on its own today and will pair with the connector when it launches.

## Install

- **claude.ai and Cowork:** find Tap Vortex in the plugin directory under Customize > Plugins.
- **Claude Code:** install from the directory, or load a local copy with `claude --plugin-dir ./tap-vortex-claude-plugin`.

## Support

Tap Vortex, Australia. https://www.tapvortex.com.au

## Licence

MIT. See [LICENSE](LICENSE).
