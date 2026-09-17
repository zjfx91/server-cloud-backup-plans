# server cloud backup: How to Get Your Server Data Off-Site Without Overpaying, With Real Plans From $4 a Month

Every sysadmin thread about server backups starts the same way: someone realizes they have a server full of data and no copy of it anywhere else. One hardware failure, one bad `rm -rf`, one ransomware payload, and it's gone. If you're searching for server cloud backup, you're probably at that stage — or past it, shopping for an actual solution.

This article covers what matters when you back up a server to the cloud: the two basic architectures, what data actually needs protecting, what it should cost, and a set of concrete plans you can order today, including agent-based Acronis backup hosted by Sharktech from $4 a month and flat-rate S3 storage at $4.90/TB for people who run their own backup software.

## What Server Cloud Backup Actually Means

Not all backups are the same thing, and picking the wrong type is the most common mistake at the buying stage.

**File-level backup** copies selected files and folders. It's cheap and simple, but it misses the things that make a server a server: the OS configuration, installed software, database state, users, cron jobs. Restoring a file-level backup means rebuilding the machine first, then copying data back onto it.

**Image-level backup** captures the whole machine — or a whole virtual machine — as one consistent snapshot. When something breaks, you restore the entire system, not just files. For servers, this is usually what you want, because downtime is dominated by rebuild time, not data transfer time.

**Object storage as a backup target** is a third approach: you keep running your own backup software (Veeam, restic, Duplicity, whatever your stack already uses) and point it at S3-compatible storage off-site. In r/sysadmin threads about physical server backups, this is the recurring recommendation — Veeam or similar software writing to an S3-compatible backend.

One thing file-level and image-level backups have in common: they need to handle **running applications** correctly. A database that's mid-write when you snapshot it can produce a corrupt copy. Serious server backup tools use application-aware processing or VSS hooks on Windows to quiesce databases before capture. If a backup product doesn't mention databases at all, treat that as a red flag for server use.

## Why the Cloud Copy Specifically: the 3-2-1 Rule

The standard framing, explained in depth by Veeam and repeated across every serious backup discussion, is the 3-2-1 rule: three copies of your data, on two different types of media, with one copy off-site.

The off-site copy is the one people skip, and it's the one that saves you when the building burns, the office gets broken into, or ransomware encrypts everything reachable on the network — including the NAS sitting on the same LAN as the server.

"Off-site" used to mean driving tapes to a bank vault. Today it means the cloud: your backup software pushes an encrypted copy to remote storage, and you never think about it again until you need it. That's what server cloud backup buys you — not more copies, but a copy that survives the failure of everything local at once.

A practical warning from the ransomware era: cloud sync is not cloud backup. Sync services replicate deletions and encryption events. If ransomware encrypts your files, a sync tool helpfully syncs the encrypted versions. Real backup keeps point-in-time versions you can roll back to.

## Two Ways to Build It

### Option 1: Managed Backup Software With Built-In Cloud Storage

You install an agent on the server, pick a schedule, and the vendor handles storage, encryption, retention, and restore. Acronis Cyber Protect is the best-known example of this category — third-party reviews describe it as managed from one cloud portal, with a single host agent handling backups alongside malware protection, web filtering, and patch management. Reviewers at Experte currently rank Acronis as the best all-round small-business cloud backup, and TechRadar's take is blunt: it's on the expensive side among consumer-grade options, but a strong pick if you need fast, enterprise-level backup and protection.

The tradeoffs are predictable. Managed services are the easiest path by far — install, schedule, forget — and the good ones add anti-ransomware features that pure storage can't offer. In exchange, you pay per GB at the vendor's rate, you're tied to their software, and restoring large amounts of data is limited by your download bandwidth.

### Option 2: Your Own Backup Software, S3-Compatible Storage as the Target

If you already run Veeam, Proxmox Backup Server, restic, or a NAS with S3 integration, you don't need anyone's agent. You need cheap, reliable, S3-compatible off-site storage. That's the niche flat-rate object storage fills: it works with any tool that speaks the S3 API, you control retention and encryption yourself, and storage-heavy setups get much cheaper per TB than managed per-GB plans.

The tradeoff is that everything is on you: schedule correctness, encryption, monitoring whether jobs actually succeed, and — the step almost everyone skips — periodically testing restores. A backup you've never restored is a hope, not a backup.

## What It Costs: Real Numbers

Vague "affordable plans" marketing copy is useless for budgeting. Here are actual current numbers.

### The Managed Route, via Sharktech

Sharktech is a hosting and cloud infrastructure provider that's been operating since 2003, with a focus on DDoS-protected infrastructure and 24/7 support. Among its services it resells **Acronis Cyber Protect backup hosting** — the managed agent-plus-storage model above — and its published pricing undercuts what you'd pay buying Acronis capacity elsewhere. Sharktech itself claims the service costs about half of competing offers, and the entry numbers are easy to verify:

- **$4/month** gets you 200GB of cloud backup storage, with additional GB at $0.02 each
- **$8 per 3 months** for the same 200GB, additional GB at $0.04
- **$12 per 6 months** for 200GB, additional GB at $0.06
- **$24 per year** for 200GB, additional GB at $0.12 — which works out to $2/month for the base block, the cheapest entry point of the four cycles

There's also an optional **Files Sync & Share** add-on at $0.03/GB monthly (with the same escalating pattern on longer cycles: $0.06 quarterly, $0.12 semi-annually, $0.24 annually) if you want sync-and-share features on top of backup.

The pricing logic is worth pausing on: longer billing cycles drop the base price but raise the overage rate. If your dataset stays near 200GB, the annual plan at $24 is the clear winner. If you expect to grow well past 200GB, monthly billing at $0.02/GB overage is the safer structure.

The Acronis service covers Windows, Linux, and macOS, physical and virtual machines, and includes ransomware protection, URL filtering, anti-malware, and patch filtering alongside the backup engine itself. Restores can be done at file level or as full system recovery, through a web interface or mobile app.

### The DIY Route: S3 Object Storage

On the storage-target side, Sharktech sells **S3 Object Storage** at a flat **$4.90 per TB per month**, with bandwidth as the only other item on the invoice (listed at $0.9/TB, and the base order configuration shows its first TB of bandwidth at $0.00). The standard order configuration is 1TB of storage plus 1TB of bandwidth for $4.90/month total. No long-term contract, no tier juggling — the rate is the same whether you store one TB or fifty.

For comparison, that's in the same territory as the budget S3-compatible providers people recommend in self-hosting communities, and well under hyperscaler list rates. The platform is triple-redundant, speaks the full S3 API, and integrates with the usual DevOps tooling — Jenkins, GitLab, Terraform — plus anything else that supports S3, which includes every mainstream backup tool.

## All Current Backup Plans, Side by Side

These are all the backup-related plans Sharktech currently displays on its product pages. Prices are in USD.

| Plan | What You Get | Base Price | Billing Cycle | Overage Rate | Purchase |
| --- | --- | --- | --- | --- | --- |
| Acronis Backup — Monthly | 200GB cloud backup, agent, anti-ransomware | $4.00 | Monthly | $0.02/GB | [ Start the 200GB monthly plan](https://portal.sharktech.net/cart.php?a=add&pid=648&configoption%5B1862%5D=200&configoption%5B1863%5D=0&billingcycle=monthly&aff=1611) |
| Acronis Backup — Quarterly | 200GB cloud backup, agent, anti-ransomware | $8.00 | Every 3 months | $0.04/GB | [ Order the quarterly plan](https://bit.ly/SharKTech) |
| Acronis Backup — Semi-Annual | 200GB cloud backup, agent, anti-ransomware | $12.00 | Every 6 months | $0.06/GB | [ Order the semi-annual plan](https://bit.ly/SharKTech) |
| Acronis Backup — Annual | 200GB cloud backup, agent, anti-ransomware | $24.00 | Every 12 months | $0.12/GB | [ Order the annual plan](https://bit.ly/SharKTech) |
| S3 Object Storage | 1TB storage + 1TB bandwidth, full S3 API | $4.90 | Monthly | $4.90/TB storage | [ Order S3 storage at $4.90/TB](https://portal.sharktech.net/cart.php?a=add&pid=643&carttpl=s3_storage_cart&billingcycle=monthly&configoption%5B1858%5D=13673&configoption%5B1859%5D=1&aff=1611) |

The Files Sync & Share add-on ($0.03/GB monthly and up, depending on cycle) can be attached to any Acronis plan.

One honest observation about the table: the quarterly, semi-annual, and annual Acronis entries are priced on Sharktech's published rate sheet but don't have individually linkable order pages, so those three point to the general order flow — the pricing you'll see at checkout matches the numbers above.

If your situation is "I need the server itself, too," Sharktech also sells the infrastructure layer: OpenStack-based public cloud hosting with a 99.999% uptime guarantee, currently starting at $39/month for a small tier (4–16 vCPU, 8–32GB RAM), scaling up through Medium at $79, Large at $249, and Enterprise at $499 per month. That's the machine your backups would be protecting — 👉 [see the cloud hosting tiers](https://bit.ly/SharKTech) if you're building from scratch.

## Which Plan Fits Which Situation

The $4 Acronis plan and the $4.90 S3 bucket solve the same problem — off-site copies — for completely different people.

**Take the Acronis plan if you don't already have backup software and don't want to become an expert in it.** At $4/month for 200GB, it's priced below most consumer cloud storage, and it includes the parts that actually require expertise: application-aware backups, versioning, encryption, ransomware detection, and a restore process you can run from a phone. For a single server or a small fleet of machines with modest data, this is the shortest path from "no backups" to "backups I can trust." The anti-malware and patch-management extras are genuinely useful if the server is exposed to the internet.

**Take the S3 bucket if you already run backup software.** If you have Veeam, Proxmox Backup Server, restic, borg with an S3 backend, or a Synology with Hyper Backup, the managed agent adds nothing but per-GB cost. Flat $4.90/TB with no contract means a 2TB server's off-site copy costs under $10/month, and your existing retention policy keeps working unchanged. This is also the better choice for archival data — old backups, compliance records, log history — where you want cheap, durable storage and rarely touch the data.

**Watch the crossover point.** Managed per-GB pricing and flat per-TB pricing converge as your dataset grows. At 200GB the Acronis plan wins easily. Past a couple of TB, do the math: Acronis overage at $0.02/GB is $20/TB, while S3 storage is $4.90/TB. Somewhere between 500GB and 1TB of active backup data, rolling your own onto object storage starts saving real money — assuming you're equipped to run it.

And remember the overage asymmetry from the table: the $24 annual plan's $0.12/GB overage is six times the monthly plan's rate. Long cycles reward stable datasets and punish growing ones.

## What to Verify Before You Commit to Any Provider

Whatever you buy — from Sharktech or anyone else — run through this checklist before the first invoice:

1. **Does the agent support your OS and hypervisor?** Acronis covers Windows, Linux, and macOS, physical and virtual. If you're on something exotic, confirm first.
2. **Does it handle your running databases?** Look for VSS support or application-aware processing if you run SQL Server, Exchange, or similar.
3. **What's the retention model?** You want point-in-time versions, not a single overwritten copy. Ransomware recovery depends on rollback depth.
4. **What does restore cost?** Storage gets billed on the way in; find out what egress or restore operations cost on the way out. Sharktech's S3 invoice has exactly two line items — storage and bandwidth — which is the model you want: predictable.
5. **Have you actually restored something?** Do a test restore within the first week of service. This is the only way to know the backup chain works end to end.
6. **Where's the encryption?** Ideally encrypted client-side before upload, so the provider can't read your data even if compelled to.

On bandwidth specifically: your backup window is bounded by your upload speed. A 1TB initial backup over a 100 Mbps uplink takes roughly a full day. Some providers seed initial backups by mail; others just let the first sync run long. Plan for it.

## Frequently Asked Questions

**Is cloud backup enough on its own?**

No, and this is the mistake worth repeating. Cloud backup gives you the off-site copy; you should still keep a fast local copy for everyday recoveries. Three copies, two media, one off-site — the cloud is the "one off-site," not the whole strategy.

**How much storage do I actually need?**

Look at your real data footprint, not your disk size. A 2TB disk with 300GB of actual data needs maybe 400–500GB of backup space once you account for retention versions. Compression and deduplication (which Acronis's service includes) shrink it further.

**Can I back up a server to consumer cloud storage?**

You can, and lots of small setups do. The problem is consumer plans are built around file sync, not versioned server images, and their clients often choke on open files and databases. If the data matters enough to back up, use a tool designed for servers.

**What happens if I cancel?**

Sharktech's backup and S3 plans run month-to-month without long-term contracts, so there's no lock-in penalty — but your stored data goes away with the service. Export anything you need before cancelling.

**Is $4/month realistic for business use?**

For a single small server, yes, genuinely. The managed plan's economics only break down at scale, which is exactly when the $4.90/TB S3 option takes over.

## The Short Version

Server cloud backup comes down to one decision: do you want the whole thing handled by an agent and a portal, or do you want cheap storage as a target for software you already run? Sharktech offers both sides of that fork with verified pricing — Acronis-managed backup from $4/month for 200GB with anti-ransomware included, or flat $4.90/TB S3-compatible storage with no contract and full API compatibility for DIY stacks.

Either way, the worst plan is the one you're still researching when the disk fails. Pick the architecture that matches your setup, run a test restore in the first week, and stop being the person in that forum thread with no backup at all.

👉 [Compare the backup plans and get started](https://bit.ly/SharKTech)
