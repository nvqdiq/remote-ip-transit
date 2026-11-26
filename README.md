# Remote IP Transit: Remote Peering, DIA, or a Blended Port? How Billing Works, How Big a Port You Need, and What to Verify Before You Sign (RPKI, DDoS, SLA)

Remote IP transit buys internet access in a region where you don't own a port. Your prefixes enter a provider's backbone at their edge near that market, and traffic to local networks leaves from there. One vendor's documentation describes it as the option for operators who would rather not "purchase a new port at the target public internet access location" and instead reach the internet in that region over the provider's backbone.

Two neighbouring products get confused with it. Remote peering puts your ASN into one or more internet exchanges without a router sitting in that exchange's colocation; you get the IXP's peering fabric instead of a full routing table. DIA is a dedicated port with committed, symmetrical bandwidth and a hard SLA, normally bought by organisations that don't run BGP at all.

What follows is how the three split in practice, how the invoice is actually calculated, what has to exist on your side before the eBGP session comes up, which technical items belong in the vendor's pack, and how SixtyNet fits. On that last point: SixtyNet publishes no per-Mbps transit rate, so its remote IP transit is a quoted line item.

> **Quick read.** Holding an ASN and running BGP? Buy transit, and buy it remote if you have no port in the target market. No ASN? DIA. Need to appear at three or four exchanges without racks in three or four cities? Remote peering. SixtyNet quotes transit per case rather than publishing a rate card, and the router licences in its catalogue are the one piece of a BGP edge it does list with prices. 👉 [Ask SixtyNet for a remote IP transit quote and see what it currently publishes](https://sixtynet.net/aff.php?aff=39).

## What SixtyNet publishes today

SixtyNet's public catalogue is hosting and software licensing. There is no listed remote IP transit SKU, no per-Mbps price, no published ASN, PoP count or transit SLA. That doesn't rule the provider out as a transit source; it means the commercial terms arrive through a quote, and the questions to put in that request are listed further down.

One catalogue item matters directly to a BGP edge you build yourself. 6WIND VSR and Virtual Accelerator licences let you terminate a full-table BGP session on a VM or white box instead of buying a hardware router. All paid tiers are perpetual with free upgrades, so the cost is one-time rather than a subscription that follows you through every renewal.

| Licence | Max throughput | IPsec tunnels / CG-NAT conntracks | Price (one-time) | Order |
| --- | --- | --- | --- | --- |
| 6WIND 10G, 7-day trial | 10.0G | 2,000 / 2,000,000 | $5.00 | [ Take the 7-day trial](https://sixtynet.net/aff.php?aff=39) |
| 6WIND 10G | 10.0G | 2,000 / 2,000,000 | $100.00 | [ Order the 10G licence](https://sixtynet.net/aff.php?aff=39) |
| 6WIND 25G | 25.0G | 5,000 / 5,000,000 | $250.00 | [ Order the 25G licence](https://sixtynet.net/aff.php?aff=39) |
| 6WIND 40G | 40.0G | 10,000 / 10,000,000 | $400.00 | [ Order the 40G licence](https://sixtynet.net/aff.php?aff=39) |
| 6WIND 60G | 60.0G | 15,000 / 15,000,000 | $600.00 | [ Order the 60G licence](https://sixtynet.net/aff.php?aff=39) |
| 6WIND 100G | 100.0G | 30,000 / 30,000,000 | $1,000.00 | [ Order the 100G licence](https://sixtynet.net/aff.php?aff=39) |
| 6WIND Unlimited | Unlimited | Unlimited | $4,500.00 | [ Order the unlimited licence](https://sixtynet.net/aff.php?aff=39) |

The rest of the public price list is VPS capacity. It has nothing to do with transit ports, and it's here so you don't mistake one for the other: the residential series ships with residential IPs and keeps ports 80 and 443 closed, which rules it out as a place to announce a web prefix.

| Plan | vCPU / RAM / NVMe | Traffic | Price | Order |
| --- | --- | --- | --- | --- |
| Residential-STARTER | 2 / 2 GB / 40 GB | 5 TB | $25.00/mo | [ Check the entry plan](https://sixtynet.net/aff.php?aff=39) |
| Residential-MICRO | 4 / 4 GB / 80 GB | 10 TB | $45.00/mo | [ Check the MICRO plan](https://sixtynet.net/aff.php?aff=39) |
| Residential-MEDIUM | 8 / 8 GB / 160 GB | 20 TB | $85.00/mo | [ Check the MEDIUM plan](https://sixtynet.net/aff.php?aff=39) |
| Residential-LARGE | 16 / 16 GB / 320 GB | 40 TB | $165.00/mo | [ Check the LARGE plan](https://sixtynet.net/aff.php?aff=39) |

## Remote IP transit, remote peering and DIA: three different jobs

The fastest way to pick is to look at what you already own. An ASN and a BGP configuration point to transit. An ASN plus a need to be visible at several exchanges points to remote peering. Neither an ASN nor BGP points to DIA.

|  | Remote IP transit | Remote peering | DIA |
| --- | --- | --- | --- |
| What you're buying | The full routing table, delivered over a provider's backbone at their edge near the target market | Presence at one or more IXPs, with peering to the networks that are members there | A dedicated port with committed, symmetrical bandwidth and an SLA |
| What you need | An ASN, BGP, and a router that can hold the full table | An ASN and BGP, plus exchange policy that accepts you as a peer | A router, nothing more; no ASN required |
| Fits when | You want to serve a region without buying a local port or rack | You want to appear at several exchanges without hardware in each city | You need guaranteed bandwidth to one site and no global routing ambitions |
| Commercial shape | Priced per Mbps committed or as a flat port price, plus cross-connect | IXP membership handled by the provider, billed through them | You pay for the committed capacity whether or not you fill it |

Remote peering's real saving isn't the port, it's everything behind it: hardware installation, colocation, utility fees, ongoing maintenance, and the administrative work of exchange membership, legal agreements and billing, which a managed provider can absorb. Remote IP transit solves a narrower problem, which is reach into one region without a local port.

Note the wording there. "Remote IP transit" is not a standardised industry term in the way "IP transit" is; treat it as one provider's way of describing remote delivery, and make any vendor define it in writing before you sign.

## How the invoice is calculated

Four billing models cover nearly every transit contract. 95th percentile sampling is the most common: the provider samples your throughput every five minutes, discards the top 5% of samples, and bills on the highest remaining one. It suits traffic that bursts. Metered billing charges for the Mbps you actually push. Flat-rate charges a fixed monthly fee for fixed bandwidth, which pays off when utilisation is steady and high. Tiered pricing moves you between bandwidth brackets.

Two commercial structures sit underneath those models and change the maths more than the model does. Per-Mbps committed pricing bills a committed rate with an overage allowance, so traffic above the commit is billed rather than blocked. Flat port pricing sells you a 10G or 100G handoff whole. Providers differ on which they'll offer, and at least one major carrier lets you choose between flat-fee and 95th percentile billing outright.

There is no public $/Mbps figure to quote you here, because transit pricing moves with bandwidth volume, with how much local infrastructure exists at the location (rural and thin-route markets cost more), with contract length (longer terms generally price lower), and with add-ons such as DDoS mitigation. Get the billing method named in the quote, and ask what happens at the commit boundary: overage billing, burst to full port speed, or a hard cap. 👉 [Ask SixtyNet to state the billing method and the overage rule in writing](https://sixtynet.net/aff.php?aff=39).

## Port size and what must exist before BGP comes up

Handoffs run from 1G through 10G, 40G, 100G and 400G. 400GE is already orderable on some networks, with native 400 Gbps interfaces across most European PoPs at one Tier-1, while other providers cap their published handoff range at 1G to 100G. Order the port for the capacity you'll need at renewal, not for the first month; upgrading a cross-connect later means a maintenance window.

The build order is fixed. First the cross-connect and port between the provider's equipment and yours, then the eBGP session over it. Nothing routes until both exist, and the cross-connect is often billed separately from the transit commit.

Your side needs an ASN, address space (yours, or leased from the provider), and a router with enough memory for the full table: over 900,000 IPv4 prefixes at last count, plus IPv6. That memory requirement is where software routing gets interesting. A 6WIND licence on a VM or white box handles the session without a hardware chassis, and because the paid tiers are perpetual, the cost doesn't recur. 👉 [See SixtyNet's router licence tiers and one-time pricing](https://sixtynet.net/aff.php?aff=39).

## What to check in a transit provider's technical pack

These are the items that separate a usable transit contract from a port that drops your routes when something goes wrong.

**RPKI.** Ask whether the provider signs its own announcements and whether it drops RPKI-invalid routes at the edge. One network drops invalid announcements and automatically opens a ticket so the customer can fix the ROA, which protects you from a mistaken announcement taking your prefix dark for hours. Others offer origin validation without stating a drop policy. Get the policy, not the feature name.

**DDoS mitigation.** Check whether it's included on every circuit or sold separately, and where scrubbing happens. Always-on edge scrubbing with no upcharge is a real differentiator; "available" mitigation services are a line item.

**BGP communities.** You want prepend, regional no-export selectors, and a blackhole community for null-routing your own prefixes during an attack. Ask for the community list before signing; some providers only hand over full documentation in the technical pack after contract.

**Blended upstreams.** A single-carrier port makes one de-peering event or one backbone failure your outage. Multi-carrier blended transit draws from several Tier-1s per PoP, and good providers let you exclude a specific carrier for legal or policy reasons. Ask which carriers serve your specific PoP, since the mix varies by location.

**Dual stack and SLA.** Confirm IPv4 and IPv6 are both delivered. On availability, published figures vary: one Tier-1 states 99.95% availability, while buyer guidance from another suggests targeting 99.99% or better. Those aren't the same promise, so ask what's measured, over what period, and what credit you receive when it's missed.

**Tier labels.** Every provider describes itself favourably and the self-descriptions contradict each other. Treat "Tier 1" as a marketing claim and evaluate the things you can verify: which carriers are in the blend, which IXPs are peered, what the SLA credits actually pay.

## Combining the three

The products aren't mutually exclusive, and the more interesting offerings merge them. One provider's combined product turns unused remote-IX capacity into transit on the same physical port, under one contract, with the full committed rate usable either way and transit allowed to burst to full port speed. Another sells remote peering as a managed service where the provider handles exchange memberships, technical support, legal paperwork and billing on your behalf.

That changes the sequencing advice. If you expect to join exchanges in a market later, ask now whether your transit port can carry IX traffic when the time comes, and whether that requires a second contract.

## Putting a SixtyNet quote together

Because the terms are quote-based, the request matters as much as the provider. Ask for the regions and PoPs available for remote delivery, the handoff sizes on offer at each, the billing model with the 95th percentile sampling window, the overage or burst rule, whether DDoS mitigation and RPKI filtering are included, the BGP community list, the contract term, the SLA with its credit schedule, and the cross-connect charge at your chosen site. Compare the answer against the same list from any other provider. 👉 [Start from SixtyNet's current catalogue and quote route](https://sixtynet.net/aff.php?aff=39).

## Which one to buy

An ASN, a BGP configuration and a target region with no port of yours: remote IP transit. An ASN and a need to be present at several exchanges without hardware in each city: remote peering, or a combined port if the provider offers one. No ASN: DIA, and don't let anyone sell you transit you can't route.

On billing, steady high utilisation favours flat rate, and spiky traffic favours 95th percentile. Before you sign, get three things on paper: the billing method with its sampling window, the behaviour at the commit boundary, and the RPKI and DDoS policies. Those three decide what the circuit costs when traffic doubles and what happens on the day someone announces your prefix by mistake. 👉 [Request SixtyNet's remote IP transit terms](https://sixtynet.net/aff.php?aff=39).
