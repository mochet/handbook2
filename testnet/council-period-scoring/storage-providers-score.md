---
description: Metrics used to compute score for storage providers working group.
---

# Storage Providers Score

## Overview

The storage providers, coordinated by the lead, have to provide a storage system which performs well in the following terms

* Low latency and reliable uploading.
* Very low probability of permanent data loss
* High upload speed.
* High upload volume capacity: many simultaneous parallel uploads.
* A high upload speed to distributors.
* A low replication latency for a new data objects to all providers for the given bag.
* A low synchronization time of new storage providers.
* Basic level of denial of service resistance at the public upload API.

Jsgenesis staff will be conducting experiments to put load on the system, including denial of service attacks, spamming, and also posing as malicious workers. **Expect Jsgenesis to rent botnet services or other denial of service infrastructure to simulate at scale attacks.**

## Knowledge Base

{% embed url="https://grizzled-corleggy-af8.notion.site/Storage-9dc5a16444934dc4bda08b596bc15375" %}
Storage Providers Working Group Knowledge Base
{% endembed %}

## Report

The working group report should include a special section with information about

* What data objects were permanently lost.
* How many bags were created.
* How many bags were deleted.
* How many data objects were uploaded, and
  * what was their total size,
  * what was the size distribution.
* Distribution of bag sizes in terms of data object counts.
* Distribution of bag sizes in terms of total data object size.
* List of failed data upload upload attempts on chain which failed due to storage bucket size being too low.
* Total capacity of all storage bucket limits.
* Total used capacity of all storage buckets.

## Score

The score is computed as follows

`[GENERAL_WG_SCORE + UPLOAD_ROBUSTNESS + UPLOAD_CAPACITY + REPLICATION_LATENCY + DOS_ROBUSTNESS]/(5*2^{N})`

where

* `GENERAL_WG_SCORE` : is computed with metric defined in [general-working-group-score.md](general-working-group-score.md "mention"). where the opportunity target is **`30%`**.
* `UPLOAD_ROBUSTNESS`: is the share of uploads of data objects where the first picked liason for a storage bucket starts accepting the upload within **`300ms`**, as measured on the client side.
* `UPLOAD_CAPACITY`: is 1, will be defined later.
* `REPLICATION_LATENCY`: is the share of uploads of data objects where the latency from upload completion to _all_ storage providers for corresponding first picked liason for a storage bucket starts accepting the upload within **`300ms`**, as measured on the client side.
* `DOS_ROBUSTNESS`: is a score computed by Jsgenesis staff for the ability to withstand DoS and DDoS attacks, which will be in the range \[0, 1], and will emphasize handling of attacks of the following kinds
  * ICMP Flooding/ Smurf Attack
  * SYN Flooding
  * Ping of Death
  * HTTP (GET/POST) Flooding
* `N` : The number of catastrophic error instances which occurred, as defined below.

### Catastrophic Errors&#x20;

#### **Permanent Data Object Loss**

A confirmed data object can no longer be recovered from storage nodes, despite not being deleted on chain.
