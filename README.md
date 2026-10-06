# Epipe service (VPWS) with Segment Routing

Functional lab of Segment Routing MPLS with IS-IS on Nokia SR OS. The SR-ISIS underlay transports three service types, built in phases: Epipe (VLL) → VPLS → VPRN. Everything runs in containers with SR-SIM 25.7.R1 and Containerlab, configured in MD-CLI/Classic.

Topology:

<img width="1020" height="187" alt="image" src="https://github.com/user-attachments/assets/193449ea-0e37-43e7-a3a8-707693ed30c4" />

Linear core with two transit nodes (P1, P2) between the PEs. All six nodes are SR-SIM; the CEs are plain SR OS routers that use the services.


                 How it works

IS-IS distributes the SIDs. Each PE/P advertises a Prefix-SID on its system /32, and its SRGB (20000–27999) in the Router Capability TLV. That is why advertise-router-capability is required. The Prefix-SID sub-TLV travels in the extended IP reachability TLV (135), so the IGP runs with wide-metrics-only.
Global labels. Label = SRGB base + index, so PE2 (index 3) is label 20003 on every node of the domain.

No LDP or RSVP for transport. SR builds the shortest-path tunnels (show router tunnel-table, protocol isis). LDP appears only in phases 1–2, and only as targeted LDP to signal the pseudowire label.
Services bind to SR-ISIS tunnels. Epipe and VPLS use SDPs with sr-isis true; the VPRN uses auto-bind-tunnel with resolution-filter sr-isis.
