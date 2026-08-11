---
title: "Blog 1"
date: 2026-08-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# REIMAGINING BINARY ASSET STORAGE IN GAMES WITH LORE

## The pain point in game development

During game development, teams often have to commit hundreds of large binary assets every day. Traditional version control systems such as Git were not built for this workload. When a binary file is modified, the system stores the entire file as a new version, regardless of how many bytes changed. This creates massive storage overhead. A 50-person studio can accumulate petabytes over a production cycle, which drives up monthly costs quickly.

To solve this problem, Epic Games created Lore, an open-source version control system with a very different approach.

## How Lore is different

Instead of treating binary files as opaque blobs, Lore breaks each file into variable-sized fragments identified by cryptographic hashes.

- If you edit a 200MB texture file, Lore stores only the fragments that contain changed bytes instead of duplicating the whole file.
- The storage model changes from linear to sub-linear. As a project grows, fragment duplication becomes more common, which saves storage.
- If the same fragment appears in 100 different textures, it is stored only once through deduplication.
- Branching across tens of thousands of unchanged assets adds almost no storage overhead, so branching is close to free.

Even better, end users such as artists and developers do not need to change their habits. They still use the familiar workflow of check out, edit, and commit, while Lore handles fragmentation behind the scenes.

## Lore architecture on AWS

To build Lore at scale, AWS and Epic designed a complete reference architecture. Data flows through the following components:

- Edge pods (Amazon EC2): Run on C8gd instances. Clients connect through QUIC, a UDP-based protocol that improves throughput and packet-loss handling. Each pod has local NVMe storage for cache.
- Write tier (Amazon ECS): Handles data durability. When push traffic arrives, the edge pods send new fragments to the write tier, which deduplicates them and stores them in S3.
- Durable storage (Amazon S3): Stores all unique fragments. These fragments are immutable, written once and read many times.
- Metadata and locks (Amazon DynamoDB): Handles file metadata, branch pointers, and locks. DynamoDB provides millisecond-level reads, which is critical when many people are competing for exclusive file locks.
- Service discovery (AWS Cloud Map): Helps edge pods find the write tier through internal DNS. When infrastructure changes, DNS updates in a few seconds without disrupting the team.

## Real-world value and personal take

After reading this article, I realized that Lore is not just a storage tool. It changes how game teams think:

- Free branching lets teams experiment with new features without worrying about storage cost.
- Studios with multiple projects can share fragments across repositories and build large shared asset libraries.

As someone learning AWS, Lore is a perfect case study in choosing the right service for the right job: S3 for low-cost durable storage, DynamoDB for fast metadata reads and writes, and EC2 NVMe for caching.

## Conclusion and deployment

Today, Lore's source code is available on GitHub, and AWS has also published an open-source Terraform module (terraform-aws-lore) to automate the full setup, from networking and compute to storage and authentication.

In the future, this architecture could expand to multi-region deployments and deeper integration with the Unreal Engine ecosystem. If you work in DevOps for a game studio, this is definitely a solution worth paying attention to.

**Reference:** https://aws.amazon.com/vi/blogs/gametech/how-lore-rethinks-binary-asset-storage-on-aws/
