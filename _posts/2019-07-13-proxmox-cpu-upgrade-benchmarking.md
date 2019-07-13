---
layout: single
title:  "Proxmox CPU upgrade & Benchmarking"
date:   2019-07-13 16:51:00 +0300
permalink:  /2019/06/proxmox-cpu-upgrade-fx-6100-to-fx-6300
categories:
  - Homelab
tags:
  - proxmox
  - virtualization
  - containers
  - benchmarking
---

Recently the shared homelab server me and a friend of mine run has run into an
issue; some of our workloads require better compute performance than we
currently have available. The server is currently running on an AMD FX-6100
processor, which we will upgrade to an FX-6300.

Looking at [Userbenchmark](https://cpu.userbenchmark.com/Compare/AMD-FX-6300-vs-AMD-FX-6100/1555vs1984) results for these two CPUs, we can expect around 12% increase in
single threaded performance. This is by no means a huge improvement, but it
should help at least a bit. Also by upgrading to a very similar processor (same
socket, same amount of processing cores) we should avoid any or most issues the
could result from the change.

To quantify the performance gains from this upgrade, we need to get a comparable
set of metrics for each cpu. To benchmark the system, a benchmarking utility
called geekbench4 was chosen. It provides easily comparable scores for both
single- and multi-threaded performance, just what we need.

Since our hypervisor of choice, Proxmox, offers two options for virtualization,
virtual machines and containers, we decided to run our tests on both to evaluate
performance differences between them.

## Installation procedure for geekbench 4

To install the benchmarking utility, first the software needs to be downloaded
from the geekbench cdn, then the archive needs to be unpacked, and after
that we are ready to begin.

    wget cdn.geekbench.com/Geekbench-4.3.3-Linux.tar.gz
    tar xf Geekbench-4.3.3-Linux.tar.gz
    cd Geekbench-4.3.3-Linux/
    ./geekbench4

## Testing methodology

The test will be started from idle with minimal services running on the host.
Both the container and the virtual machine are running on solid state storage
and have 4 cores available for use. Four cores, rather than the full six, were
chosen as it provides a realistic scenario for a vm with high compute load in
our use case. This might result in variance of the results based on how the
hypervisor distributes available resources in each test case, but since running
all six cores is not a realistic workload, this variance should be accepted.

The tests are run on fully updated Ubuntu 18.04.2 LTS systems. All tests were
run twice to confirm that the results were not significantly different from each
other.

## Test results

Let's first take a look at the single-core performance. This is the most important
metric for us.

<link rel="import" href="/assets/html/img-format.html">
<img class="center" src="/assets/2019-07-13/SingleCorePerformance.png" width="80%">

We see an overall increase of 14,3% in virtual machine single core performance
and an 11,4% increase in container performance. When comparing the performance of
virtual machines to that of containers, we see virtual machines taking the lead
by a very small margin, so there's no clear winner, at least yet.

Let's see if that changes when we take a look at the multi-core performance
numbers.

<link rel="import" href="/assets/html/img-format.html">
<img class="center" src="/assets/2019-07-13/MultiCorePerformance.png" width="80%">

We can again see container performance being slightly behind that of virtual
machines. We see an increase of 12,7% on the virtual machine side, and what is
quite surprising, an increase of 18,1% on the container side. This might be just
some weird anomaly, but the difference is still notable. This might indicate better
multi-core performance scaling on containers as opposed to virtual machines, but
further testing would need to be conducted to rule out this being just a fluke.

In conclusion, the performance increased about as we expected. We also noticed
that, in our testing, containers were slightly behind in performance in all test
cases. This seems a bit counter-intuitive, as containers are supposed to work
with less overhead than virtual machines. However, containers still have
advantages that make them worth the small performance hit, for example, faster
boot times. These advantages over "traditional" virtual machines might be
something I'll cover later in better detail, but that's it for now.
