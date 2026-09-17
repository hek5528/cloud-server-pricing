# cloud server: How It Works, What It Really Costs, and How to Pick a Plan That Fits Your Workload

Most people who search "cloud server" aren't looking for a definition. They're trying to figure out one of three things: what they're actually buying, what it will cost per month, or which provider and plan won't burn them six weeks later with a surprise invoice. Those three questions are connected, and the answers mostly come down to how a given platform handles pricing, resource limits, and outbound data.

This guide walks through all of it — how cloud servers differ from VPS and dedicated machines, where bills typically go wrong, and a full breakdown of one concrete option: Sharktech's OpenStack-based Public Cloud, including every tier, every resource cap, and the overage rates that apply when you exceed them.

## What a cloud server actually is — and when it beats a VPS

A cloud server is a virtual machine running on pooled infrastructure. Compute, storage, and networking are spread across multiple physical servers and storage nodes, and your VM lives on that shared fabric rather than being tied to one specific box. If a node fails, your workload keeps running elsewhere. That's the core technical difference from a traditional VPS, where your virtual machine sits inside a single physical server — and if that server dies, you're offline until someone fixes it.

The second difference is billing and scaling. A VPS is usually a fixed slice: 4 cores, 8 GB RAM, 200 GB disk, one monthly price. A cloud server, on a platform like Sharktech's, is closer to a resource pool. You get an allocation — say 8 vCPUs, 8 GB RAM, and 300 GB of SSD — and you can carve that pool into as many virtual machines as you want, in any combination. One 8-core VM, or eight 1-core VMs, or anything in between.

When does a cloud server make sense over a VPS?

- **You need to scale up and down.** Traffic spikes, batch jobs, staging environments that come and go. A cloud server lets you allocate and release resources without migrating to a different product.
- **You run multiple small services.** Splitting one pool across several VMs beats managing several separate VPS subscriptions.
- **Uptime matters more than rock-bottom price.** Redundant infrastructure with automatic failover costs more than a cheap single-node VPS, but it survives hardware failures.
- **You want API-driven infrastructure.** Cloud platforms generally expose full REST APIs (Sharktech's covers Nova for compute, Cinder and Swift for storage, Neutron for networking, Keystone for identity), which matters if you automate deployments.

If you just need one always-on box for a small website at the lowest possible price, a VPS or even shared hosting is still the more sensible buy. A cloud server is a tool for workloads that change shape.

## The two pricing models that decide your bill

Cloud pricing confuses people because providers mix two models, sometimes on the same page.

**Fixed presets.** You buy "2 vCPU / 4 GB / 80 GB" and that's what you get. Simple, but every change means switching products, and bursts above your limit get throttled or rejected.

**Resource pool with metered overage.** You buy a base allocation, and anything you consume beyond it is billed hourly. Sharktech's Public Cloud works this way: each plan includes a fixed amount of resources, and once you exceed those limits, you pay only for the extra, at published hourly rates. The formula, straight from Sharktech's own documentation:

$$\text{Included Resources} \times \text{fixed fee} + \text{extra consumption} \times \text{hourly rate} = \text{total monthly fee}$$

One detail worth knowing: on Sharktech's Small, Medium, and Large tiers, the overage is capped — each plan has a maximum resource ceiling, so a runaway script or a misconfigured auto-scaler can't quietly multiply your invoice. The Enterprise tier and custom configurations are uncapped instead, trading that safety net for headroom.

The alternative model is **fixed monthly billing** (what Sharktech calls Dedicated Cloud): you prepay for an exact set of resources and the invoice never moves. Same infrastructure, different invoice. More on that trade-off below.

## Egress fees: the line item that wrecks cloud budgets

Outbound data transfer is where cloud bills go sideways. Most major providers charge for egress by the gigabyte, and at hyperscaler rates the numbers add up fast — Oracle's own cloud economics comparison puts 1 TB of outbound transfer at roughly $54 on OCI and around $145 on AWS. Serve a lot of media, run a download service, or back up large datasets off-platform, and egress can quietly outgrow your compute costs. It's also the mechanism that makes leaving a provider expensive: getting *your own data out* costs money.

Sharktech's approach here is unusually simple. Inbound traffic is free and unlimited. Every Public Cloud plan includes 20 TB of outbound transfer, and overage is billed at **$0.002 per GB** — which works out to about $2 per additional terabyte (Enterprise tiers get a lower overage rate of $0.0015/GB). Compared against the $54–$145-per-TB range above, that's the kind of arithmetic that matters for bandwidth-heavy workloads: file hosting, streaming origins, CDN sources, backup targets.

Sharktech's marketing pages claim savings of 40%–80% versus hyperscalers depending on which page you read. Treat those as vendor claims until you price out your own workload — but the egress math alone is checkable, and it checks out.

## Sharktech cloud server plans: every tier, every cap

Sharktech has been around since 2003, originally built around DDoS-protected hosting, and its cloud runs on OpenStack with Virtuozzo's hybrid infrastructure stack underneath. The Public Cloud product line currently shows four priced tiers plus a custom option. Here's the full picture, pulled from the live order system:

| Plan | vCPU (included – max) | RAM (included – max) | SSD Storage | Bandwidth | IPv4 | Price | Get it |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Small** | 4 – 16 | 8 – 32 GB | 300 – 2,400 GB | 20 TB + overage | 1 free | **$39.00/mo** | [ Deploy the Small tier](https://portal.sharktech.net/aff.php?aff=1611&pid=602) |
| **Medium** | 8 – 32 | 16 – 64 GB | 800 – 6,400 GB | 20 TB + overage | 1 free | **$79.00/mo** | [ Deploy the Medium tier](https://portal.sharktech.net/aff.php?aff=1611&pid=603) |
| **Large** | 32 – 128 | 64 – 256 GB | 1,500 – 12,000 GB | 20 TB + overage | 1 free | **$249.00/mo** | [ Deploy the Large tier](https://portal.sharktech.net/aff.php?aff=1611&pid=604) |
| **Enterprise** | 64 – unlimited | 128 GB – unlimited | 5,000 GB – unlimited | 20 TB + overage | 1 free | **$499.00/mo** | [ Deploy the Enterprise tier](https://portal.sharktech.net/aff.php?aff=1611&pid=605) |
| **Custom** | Configured to spec | Configured to spec | NVMe / SSD / HDD mix | Custom | Custom | Quoted by sales | [ Request a custom quote](https://bit.ly/SharKTech) |

A few things the table doesn't fully show:

- **HDD and NVMe storage are also available** beyond the included SSD. On Small–Large tiers, you can add up to 4,800–24,000 GB of HDD and 1,200–6,000 GB of NVMe depending on the tier, billed only if you use it.
- **Overage rates** (Small–Large): CPU $0.0025/hr per core, RAM $0.0035/hr per GB, SSD $0.00006/hr per GB, NVMe $0.00009/hr per GB, HDD $0.00002/hr per GB. Enterprise tiers get discounted overage rates (CPU $0.002/hr, RAM $0.003/hr, SSD $0.000045/hr).
- **Extra IPv4 addresses** cost $1.50/month each beyond the first free one; up to 16 on Small–Large, unlimited on Enterprise.
- **Included at no extra cost**: security policies, load balancing, network management, routing, and Kubernetes support — all listed as included services on every tier.
- **Locations**: Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam, selectable at checkout with no price difference between regions.
- **Billing cycles**: monthly is the default; the order page lists automatic discounts for longer commitments (5% quarterly, 10% semi-annual, 15% annual).

Want to poke at the numbers yourself before committing? 👉 Open the live plan configurator and price your exact resource mix.

## How a resource pool plays out in real life

The pool model is easier to grasp with a worked example, using the published rates above.

Say you're on the **Large tier** ($249/mo, 32 cores / 64 GB RAM / 1,500 GB SSD included) and you spin up six VMs, each configured with 8 cores and 16 GB of RAM, running 24/7. Total consumption: 48 cores and 96 GB of RAM. That's 16 cores and 32 GB over your included allocation.

The overage cost, at the published hourly rates over a 30-day month (720 hours):

$$16 \times \$0.0025 \times 720 = \$28.80 \quad \text{(CPU)}$$

$$32 \times \$0.0035 \times 720 = \$80.64 \quad \text{(RAM)}$$

Total bill: $249 + $28.80 + $80.64 = **$358.44** for the month — and the six-VM setup used zero extra storage, so no disk overage. If some of those VMs only ran during business hours, the overage would shrink accordingly, because metering is hourly, not monthly-flat.

That's the whole appeal of the model: you don't pick a plan per-VM, you pick a plan per *total workload*, and short-lived machines cost proportionally less. The flip side is that the bill moves with usage, which is exactly why the resource caps on Small–Large exist. If you need the mental simplicity of "same invoice every month," that's what the Dedicated Cloud billing model is for.

## What's under the hood: OpenStack, storage tiers, and networking

The platform details matter more than most buying guides admit, because they determine what you can actually do after checkout.

**OpenStack foundation.** The stack is open-source and vendor-neutral. Practical consequences: no proprietary lock-in, full REST API access for automation, and — notably — you can upload your own VM disk images (including custom ISOs and qcow images) and *download your server images at any time*. That second part is rarer than it should be. If you decide to leave, you can take your images with you, which removes the usual exit-cost asymmetry that keeps people stuck on platforms they've outgrown.

**Multi-tier storage with published performance estimates.** Official figures per volume: SSD at roughly 350 MB/s and 6,000 IOPS, NVMe at 1.2 GB/s and 18,000 IOPS, HDD at 120 MB/s and 3,000 IOPS (sequential read/write; Sharktech notes results vary with technical factors). The practical read: SSD is fine for general workloads, NVMe is where databases and I/O-heavy applications should live, and HDD is the cheap tier for archives and backups. Third-party testing by HostAdvice found the NVMe layer delivered around 5 GB/s sequential reads in their benchmarks — hyperscaler territory, in their words.

**Networking.** The cloud backbone runs on 40G/100G links with built-in DDoS protection — consistent with the company's origins in DDoS mitigation. You get private networks for inter-VM traffic, security groups (firewall rules), load balancers, virtual routers with NAT, floating IPs, native VPN for hybrid setups at no charge, and both IPv4 and IPv6. HostAdvice's testing measured roughly 10 Gbps download and over 20 Gbps upload between Sharktech locations, with 0.17 ms idle latency — near the physical floor for intra-datacenter traffic.

**OS images.** Official Linux cloud images from major distributions, refreshed weekly, with SSH key injection, cloud-init/user-data scripting support, and snapshot scheduling.

**Backups.** An Acronis Cloud Backup add-on is offered during checkout starting at $4/month for 200 GB of storage, with additional GBs at $0.02.

## Public Cloud vs Dedicated Cloud: same hardware, different invoice

Sharktech splits its cloud into two billing models on identical infrastructure. Public Cloud is the pay-as-you-go version described above: fixed base + hourly overage, with caps on Small–Large. Dedicated Cloud is the prepaid version: you order an exact resource allocation and pay a fixed amount monthly — if you pay for 8 cores, you get 8 cores, no more, no less, and the invoice never varies. Dedicated tiers run from extra-small up through the largest sizes, but they're quoted through sales rather than listed at fixed public prices, since the allocation is configured per order.

The choice is really about your workload's shape:

- **Spiky or unpredictable usage** → Public Cloud. You keep headroom available without paying for it 24/7.
- **Steady, forecastable usage** → Dedicated Cloud (or an annual Public Cloud commitment). Fixed costs are easier to budget, and there's no metering to watch.
- **Compliance or procurement teams that hate variable invoices** → fixed billing, full stop.

Not sure which side of that line you're on? 👉 Book Sharktech's free cloud consultation and have their team size it with you.

## Which tier should you actually pick

Based on the verified specs, some straightforward guidance:

- **Small ($39/mo)** covers testing environments, small apps, a couple of lightweight production VMs, or your first dip into running your own cloud. The included 4 cores / 8 GB can stretch to 16 cores / 32 GB when needed, which is real headroom for the price.
- **Medium ($79/mo)** fits growing production workloads — a small fleet of app servers, a database with room to breathe, or a dev/staging/production split inside one pool.
- **Large ($249/mo)** is where multi-service production setups land: enough included resources to run a proper environment (32 cores, 64 GB, 1.5 TB SSD) without touching overage at all.
- **Enterprise ($499/mo)** is for serious compute demands, and it's the tier where the caps come off — resources scale without ceiling, and the overage rates drop (bandwidth overage falls to $0.0015/GB). This is the one to pick if you genuinely can't predict consumption.
- **Custom** exists for configurations that don't fit the ladder, including heavier storage or network allocations.

One honest note on sizing: the hourly overage rates are cheap enough that starting one tier lower than you think you need is usually the cheaper mistake. Scaling up mid-month on Public Cloud doesn't require redeploying — resources adjust in the panel.

If you're ready to compare the configurations side by side, 👉 open the full Public Cloud plan list and pick your region.

## The fine print worth knowing before you deploy

The details that don't make the headline but affect the experience:

- **No general money-back guarantee.** HostAdvice's review notes that Sharktech payments are non-refundable, with the only exception being billing disputes raised within 30 days that Sharktech upholds — and those result in account credit rather than a refund. There's no free trial either, though hourly metering means a test workload can cost cents. Don't commit annual pricing until you've run the service for a month.
- **Payment options are broad**: credit cards, PayPal, wire transfers, Western Union, and Alipay — handy for international buyers.
- **Support is 24/7** via tickets, with phone access (a rarity at this price level). HostAdvice's independent test logged a 39-minute ticket response at 1 AM. Their reviewer's caveat: answers to advanced tuning questions can be general, assuming you have the technical background to implement them. This is a self-managed service — you get the infrastructure, not a managed operations team.
- **Uptime guarantee: 99.999%** on the Public Cloud per Sharktech's pricing page. The general network SLA sits at 99.99%, with the higher figure applying to the cloud platform.
- **Third-party standing**: HostAdvice's expert review scores Sharktech Public Cloud at 9.4/10 overall, and the company holds HostAdvice uptime and service awards for 2026. Trustpilot sits at 3.5/5 across a small sample of 13 reviews — a mixed but limited dataset, so weight the detailed expert reviews more heavily than the aggregate score.
- **Data centers are US-centric** (four US locations plus Amsterdam). If your users are in Asia or South America, latency will reflect that geography. Pick the region closest to your users at checkout.

## The short version

A cloud server is the right tool when your workload changes shape — and the buying decision comes down to three numbers: the base plan price, the overage rates, and the egress rate. Sharktech's Public Cloud is competitive on all three ($39 to $499 monthly bases, CPU overage at $0.0025/hr, egress at $0.002/GB with 20 TB included and free inbound), runs on OpenStack with no lock-in and downloadable images, and throws in DDoS protection, Kubernetes, load balancing, and a 99.999% uptime guarantee at every tier. The trade-offs: no refunds, self-management, US-heavy regions, and a small Trustpilot sample.

Start on the tier that matches what you'd run *today*, watch the usage panel for a month, and let the hourly metering tell you where to go next. 👉 Deploy a cloud server and see how the numbers work out for your actual workload.
