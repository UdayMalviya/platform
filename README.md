# platform

Internal developer platform on k3s: GitOps deploys, PR previews, and infrastructure rebuilt from code.

**S1** is the name of this project: a small internal developer platform (IDP) that I am building to learn how deployment platforms work. This repository holds its infrastructure code, deployment configuration and documentation.

**Status:** Week 1 of 6 in progress.

## Problem

Small teams ship applications faster than they can operate them. Three problems hurt most:

1. **Getting live is hard.** Putting an app on the internet needs containers, networking, TLS certificates and DNS knowledge all at once.
2. **Changing safely is hard.** When deployment is manual, a bad release is stressful to undo, and there is no safe place to test a change before it reaches users.
3. **Recovering is slow.** If a server is lost, rebuilding it by hand takes hours, because the setup was never written down. Scaling for more traffic adds more setup, cost and networking work.

For a solo developer or a small team, this operational work competes with building the product and serving clients.

## Who it's for

- Solo developers who can build an app but struggle to deploy and operate it.
- Small development teams without a dedicated operations or platform engineer.

The person using S1 needs to know Git and how to write an app. They do not need to know Kubernetes.

## Why not Heroku, Render, Fly.io or Vercel?

Managed platforms already solve this problem well, and for many teams they are the right choice. I am building S1 for three reasons: to learn how such platforms work internally, to keep full control and portability (no vendor lock-in), and to prove the whole platform can run on free tiers with no paid services.

## Goal

S1 lets a developer deploy an app without learning Kubernetes (the system that runs and manages containers; **k3s** is a lightweight version of it).

A developer pushes code and gets a live URL. Every pull request gets its own temporary preview URL, which is removed when the PR closes. A bad release is undone with `git revert`, not by typing commands on a server. The platform itself is created from code, so if it is lost it can be rebuilt (target: about 30 minutes, to be measured in Week 1). Failed instances restart automatically, and apps scale when load increases.

S1 hides the work of managing servers, networking and Kubernetes resources from application developers. The platform builder (me) still has to understand and operate that work.

Success is measured, not claimed: time from push to live, time to rebuild the platform from scratch, and time to detect and recover from each failure drill.

## Definition of done

1. This README, with an architecture diagram and a decisions section that explains why each major choice was made.
2. A live demo and a 2 to 3 minute video that shows a push going live, a PR preview and a rollback.
3. Measured results: push-to-live time, full rebuild time, and time to recover for each failure drill.
4. A one-page writeup or blog post.

## Constraints

- No paid services; free tiers only.
- 6 weeks plus 1 buffer week, at 10 to 20 hours a week.
- Pre-decided cuts if time runs short, in this order: the portal's "create from template" stretch, network policies, multi-node (fall back to a single node, with trade-offs documented).

## Milestones

1. **Cluster as code:** the cluster can be destroyed and rebuilt from this repo in about 30 minutes.
2. **GitOps pipeline:** a push to `main` deploys with no manual step, and `git revert` rolls it back.
3. **Ingress, TLS and previews:** opening a PR posts a working preview URL; closing it cleans everything up.
4. **Secrets, scaling and guardrails:** encrypted secrets, autoscaling proven by a load test, and default-deny network rules.
5. **Developer portal:** service state is visible without using `kubectl`.
6. **Failure drills and documentation:** each drill has a short report with time to detect and time to recover.
7. **Buffer week** for anything that slipped.

## Known debt

None yet.