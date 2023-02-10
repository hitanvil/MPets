# What is MPets
MPets is a Multi-core Periodic EvenTs Scheduler. It is high performance and low overhead to support many realtime application.

# Design Motivation
BS (Base Station) in cellular communcation provides the portal for mobile phone to connect to Internet. It should process data in a periodic way. For example, 4G/LTE or 5G/NR BS outputs digital samples to RRU (Remote Radio Unit) every TTI (Transmission Time Interval, 1 ms for 4G, 500us or even smaller for 5G) for transmission, and receives digital samples from RRU every TTI. 

The processing includes physical layer (L1) processing and upper layer (L2+) processing. All L1 processing and partial L2+ processing need be done in one or few TTIs. The strigent time requirement is a big challenge for general purpose CPU (GPCPU). 

More challenging, the processing load is very high that one core of GPCPU cannot handle it. It is necessary to distribute those workload to multiple cores on CPU so that they can run in [parallel](https://en.wikipedia.org/wiki/Parallel_computing) to reduce processing latency and meet realtime requirement.

# Design Consideration and Methodology
As described above, the workload is very high and new workload comes in a periodic way, so most of time CPU is occupied by the workload. The classic interrupt-based design methodology is not suitable now. A run-to-complete, polling-based design methodology is adopted here. The same design methodology is already used in well-known [DPDK](https://www.dpdk.org/) for packet processing.

