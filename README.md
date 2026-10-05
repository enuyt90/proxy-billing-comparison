# proxy-seller: how to compare pay-per-GB and per-IP billing, test a pool before you scale, and pick the plan that fits

Two people type the same phrase into a search box and want opposite things. One is running a scraper that burns 400 GB a month and needs the lowest reliable rate. The other has a shortlist of five providers open in tabs and wants a reason to believe the "90 million IPs" claim on someone's sales page. The pages usually ranking for this term serve neither of them well — most are affiliate lists stitched together from other affiliate lists, and the rest are vendor homepages with a pricing widget bolted on.

What follows is the middle part most of those pages skip: how proxy sellers actually bill you, which numbers on a pricing page are worth trusting, and how one provider's plans translate into cost per successful request rather than cost per gigabyte.

## The billing model decides your bill more than the headline price

A $1-per-GB rate and a $0.75-per-IP rate are not comparable numbers, even though both look cheap next to each other. Two models dominate the market, and the choice between them is usually made by your traffic pattern, not your budget.

**Per-GB (bandwidth) billing** is standard for residential and mobile pools. You prepay for a volume of traffic and it drains as data passes through the proxy. Residential IPs cost what they cost because they come from real household connections, which are harder to source than server space. If your workload is spiky — heavy during an audit week, near zero the rest of the month — this model protects you from paying for capacity you never use.

**Per-IP (rental) billing** is standard for datacenter and ISP products. You rent a static address for a fixed period, usually a month, and traffic is typically unmetered. This wins when you need the same address to persist: an account login that must come from the same place every day, a monitoring job where IP churn breaks the session, anything where identity stability matters more than volume.

The failure mode is predictable. Teams buy per-IP because the price per unit looks small, then discover their actual workload is bandwidth-shaped, or the reverse. A hundred static IPs at sub-$1 each is a bargain right up until you only needed six of them.

### Where a hybrid seller fits, and where it doesn't

Proxy-Seller, the Cyprus-based provider operating since 2014, splits the difference by selling both: dedicated IPv4 and IPv6 addresses billed per address with unmetered traffic, static ISP proxies on the same model, and rotating residential billed per gigabyte. It publishes 47M+ consent-sourced IPs across 220+ locations, ISO 27001 and ISO 9001 certification, and a 99.7% uptime figure. Its published residential entry rate varies by tier and by which third-party page you read, so treat any specific number you see quoted as needing a check against the live page.

That structure suits teams that want dedicated addresses and don't mind per-address math. It's a weaker fit if you need the largest residential pool on the market or if you're hitting heavily defended targets — its own material positions it for mid-market data teams rather than enterprise scraping at the extreme end.

Which brings up the more useful question: what is your workload actually shaped like?

## What a price per GB is really buying

The rate on the pricing page reflects four things, and only one of them is the number itself.

**Pool size and sourcing.** A pool built from consent-based, first-party sources behaves differently from a resold one. Companies that build their own pool tend to say so loudly, because it's expensive to do. DataImpulse, for instance, advertises 90M+ ethically sourced IPs across 195 countries and describes itself as first-party rather than a reseller. Whether the pool is genuinely first-party is testable — you measure exit-node freshness across sessions.

**Targeting granularity.** Country-level targeting is table stakes and usually free. City, ZIP, and ASN targeting is where vendors make their margin back. This is the single most commonly missed cost driver in proxy budgeting, and it deserves its own section below.

**Expiry terms.** Monthly billing with unused-traffic forfeiture is the industry default. Some providers let purchased bandwidth sit indefinitely. That difference matters enormously for intermittent workloads — if you buy 100 GB in March and use 30 GB, forfeiture means you effectively paid triple.

**Concurrency and success rate.** A 70% success rate at $1/GB costs more per usable page than a 95% rate at $1.50/GB. Cost per successful request is the only metric that survives contact with production.

## Five checks before you send anyone money

Spending the first hour on a repeatable test saves the argument later. The order matters less than actually running all five.

1. **Verify the ASN, not the label.** Pull an exit IP and look up its owner. A "US residential" pool that resolves to a hosting ASN is not a residential pool. For mobile products, the ASN should belong to a carrier, and the address should sit behind carrier-grade NAT like every real mobile IP does.
2. **Count unique exits, not advertised endpoints.** Hit 300 requests through a rotating endpoint and log how many distinct IPs you actually saw.
3. **Test at production concurrency.** If your job runs 50 threads, test at 50. Bad inventory tends to fail in clusters tied to a subnet, not as random single errors.
4. **Check geo consistency over time.** Pick a target country and confirm the IP doesn't drift into a different region between sessions.
5. **Read the refund terms before you need them.** Windows are short and conditioned. Keep raw logs, timestamps, request counts, and error codes — a replacement request backed by "22% timeouts at 50 threads on subnet X" resolves faster than "the proxies are bad."

> A pool that fails check one or two isn't a cheap option. It's the same price with a hidden retry multiplier attached, and retries consume the exact bandwidth you already paid for.

## DataImpulse's full plan list, all four product lines

DataImpulse runs on pay-as-you-go top-ups with no subscription, and purchased traffic doesn't expire. The minimum purchase across every product line is $5, which is what makes the entry tiers genuinely testable rather than a demo.

| Product line | Plan | Traffic | Price | Effective rate | Billing | Purchase |
| --- | --- | --- | --- | --- | --- | --- |
| Residential | Intro | 5 GB | $5 | $1.00/GB | One-time top-up | [ Start with 5 GB of residential traffic for $5](https://bit.ly/dataimPulse) |
| Residential | Basic | 50 GB | $50 | $1.00/GB | One-time top-up | [ Get the 50 GB residential pack](https://bit.ly/dataimPulse) |
| Residential | Advanced | 1 TB | $800 | $0.80/GB | One-time top-up | [ Take the 1 TB residential tier at $0.80/GB](https://bit.ly/dataimPulse) |
| Residential | Custom+ | 5 TB+ | From $4,000 | Custom | Negotiated | [ Talk to sales about 5 TB+ residential volume](https://bit.ly/dataimPulse) |
| Datacenter | Intro | 10 GB | $5 | $0.50/GB | One-time top-up | [ Test datacenter proxies from $0.50/GB](https://bit.ly/dataimPulse) |
| Datacenter | Basic | 100 GB | $50 | $0.50/GB | One-time top-up | [ Buy 100 GB of datacenter traffic](https://bit.ly/dataimPulse) |
| Datacenter | Advanced | 1 TB | $450 | $0.45/GB | One-time top-up | [ Get the 1 TB datacenter tier](https://bit.ly/dataimPulse) |
| Datacenter | Custom+ | 5 TB+ | From $2,250 | Custom | Negotiated | [ Request datacenter pricing for 5 TB+](https://bit.ly/dataimPulse) |
| Mobile | Intro | 2.5 GB | $5 | $2.00/GB | One-time top-up | [ Try mobile proxies for $5](https://bit.ly/dataimPulse) |
| Mobile | Basic | 25 GB | $50 | $2.00/GB | One-time top-up | [ Get 25 GB of mobile traffic](https://bit.ly/dataimPulse) |
| Mobile | Advanced | 1 TB | $1,600 | $1.60/GB | One-time top-up | [ Take the 1 TB mobile tier at $1.60/GB](https://bit.ly/dataimPulse) |
| Mobile | Custom+ | 5 TB+ | From $8,000 | Custom | Negotiated | [ Ask about 5 TB+ mobile volume](https://bit.ly/dataimPulse) |
| Premium residential | Intro | 1 GB | $5 | $5.00/GB | One-time top-up | [ Test premium residential at $5/GB](https://bit.ly/dataimPulse) |
| Premium residential | Basic | 10 GB | $50 | $5.00/GB | One-time top-up | [ Get 10 GB of premium residential traffic](https://bit.ly/dataimPulse) |
| Premium residential | Custom+ | 5 TB+ | From $20,000 | Custom | Negotiated | [ Discuss premium residential at scale](https://bit.ly/dataimPulse) |

Two notes on that table. Volume discounts of roughly 20% apply at the 1 TB tier on residential and mobile. And there's no free tier — every plan starts with a paid top-up, though Intro plans carry a 7-day money-back guarantee on card payments if less than 80% of the traffic has been used. Crypto purchases on Intro plans are not refundable, which is worth knowing before you pay that way.

## The 2× multiplier that pricing pages bury

Here's the line that changes budgets. On DataImpulse's standard residential plans, country-level targeting is included in the base rate. Everything finer — state, city, ZIP, ASN — is billed at double the standard per-GB rate. Budget $1/GB, use city targeting, and you're at $2/GB.

The datacenter product page lists state, city, ZIP, and ASN targeting as included features, which would make batch SERP work considerably cheaper than residential. That's an inconsistency worth confirming with support before you build a budget around it, because the difference between $0.50/GB and $1.00/GB compounding over a terabyte is not small.

Concurrency limits aren't published as a hard number either. If your pipeline depends on thousands of parallel requests, ask before you commit rather than after.

## How to spend the first $5 properly

The $5 entry point is the most useful part of the whole pricing structure. Five dollars buys 5 GB of residential, 10 GB of datacenter, or 2.5 GB of mobile, so you can test the tier that matches your job without a subscription to cancel.

A sequence that produces useful data rather than a vague impression:

1. Buy the smallest top-up in the product line you actually need. Match the tool to the job first — datacenter for unguarded bulk work, residential for anything enforcing IP reputation, mobile only when you genuinely need carrier fingerprints.
2. Configure one sticky and one rotating port and compare. Sticky sessions hold an IP for a set window, configurable between 1 and 120 minutes; rotating changes the exit on every request. Picking the wrong one for your target produces failures that look like a pool quality problem when it's a configuration problem.
3. Run 200–300 requests against your real target at real concurrency. Log latency, success rate, timeout ratio, and whether each exit's geo matched what you requested.
4. Do the same run against a second provider's trial. The comparison is the point.

Independent benchmarking gives you a rough baseline to test against. Proxyway's 2025 market research, cited in third-party reviews of the provider, reported over 300,000 unique proxies in the regular US pool and success rates in the 70–75% range on aggressively defended targets like Google and Instagram. On ordinary commercial sites the numbers run higher. TechRadar's review reported consistently high success rates in its own testing. DataImpulse publishes a 99.51% success rate and a 4.8/5 G2 score, both of which are the company's own figures rather than independently verified measurements — treat them as a claim to test, not a result.

Setups vary by tool. Most clients accept the standard `host:port:username:password` credential format, which is what makes DataImpulse workable in antidetect browsers, scraping frameworks, and headless automation without custom glue code. The trade-off is deliberate: there's no scraping API. You get raw proxy connections and write your own request handling, retries, and parsing. If you want a managed unlocker that returns parsed HTML, this isn't it.

> One restriction to plan around: UDP traffic is supported but has to be enabled manually through support. Budget a day for that if your workflow needs it.

## Where DataImpulse is the wrong answer

Being clear about this saves everyone time. Skip it if you need a bundled scraping API with parsing and CAPTCHA handling built in — it isn't offered. Skip it if you need a very small number of static, permanently assigned IPs for account work, since its products are metered bandwidth pools and rotating endpoints rather than per-address rentals; a per-IP seller like Proxy-Seller is the more natural shape for that. And skip it if you need city or ASN targeting at high volume without absorbing the 2× rate.

Where it lines up: teams whose monthly usage swings, who want to stop forfeiting unused bandwidth, and who are comfortable writing their own request layer. The non-expiring traffic is the structurally interesting part. Most providers either bill monthly or expire what you bought; if your workload runs in bursts around audits, campaigns, or reporting cycles, that single design choice outweighs a few cents per gigabyte.

## Questions that come up

**Is there a free trial?** No. Every plan starts at $5. The 7-day refund window on Intro plans is the closest equivalent.

**Can I mix product lines on one account?** Yes, all four run under the same pay-as-you-go balance, so routing local rank checks through residential and bulk audits through datacenter doesn't require separate vendors or invoices.

**How many locations are covered?** DataImpulse lists 195 countries across the residential pool. Datacenter and mobile coverage is narrower. Check your specific target country before buying a tier.

**Does unused traffic really never expire?** Per the provider's stated policy, yes — purchased GB stay on the balance until consumed. Budget accordingly rather than assuming it, and confirm on your own account before committing to a terabyte.

## Bottom line

The useful version of this search isn't "who sells proxies." It's "which billing model matches how my traffic actually behaves, and which seller can prove their pool is real." Answer the first question before comparing rates, because per-GB and per-IP numbers don't sit on the same axis. Then test inside a refund window and measure cost per successful request.

If your work is bandwidth-shaped, bursty, and you're building your own request layer, the $5 entry tier is enough to find out whether the pool holds up. [👉 Five dollars buys 5 GB of residential traffic with no subscription and no expiry](https://bit.ly/dataimPulse) — a cheaper answer than signing a monthly contract and discovering the IPs were datacenter addresses all along.
