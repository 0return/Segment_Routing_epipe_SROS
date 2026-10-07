# Epipe service (VPWS) with Segment Routing

Functional lab of an Epipe (VLL/VPWS) transported by Segment Routing MPLS with IS-IS. Everything runs in containers with SR-SIM TiMOS-B-25.10.R2 and Containerlab, configured in MD-CLI/Classic.

Topology:

<img width="1271" height="307" alt="image" src="https://github.com/user-attachments/assets/5bf22efe-a18b-4389-bfe1-bfd75c5ac082" />


Linear core with two transit nodes (P1, P2) between the PEs. All six nodes are SR-SIM; the CEs are plain SR OS routers at both ends of the Epipe.


                 How it works

IS-IS distributes the SIDs. Each node advertises a Prefix-SID on its system /32, and its SRGB (20000–27999) in the Router Capability TLV. That is why advertise-router-capability is required. The Prefix-SID sub-TLV travels in the extended IP reachability TLV (135), so the IGP runs with wide-metrics-only.
Global labels. Label = SRGB base + index, so PE2 (index 4) is label 20004 on every node of the domain.

SR is the transport. No LDP on the links and no RSVP. SR builds the shortest-path tunnels (show router tunnel-table, protocol isis), and the SDP binds to them with sr-isis true.
T-LDP only signals the service label. LDP runs on the PEs with no interfaces; the targeted session toward the SDP far-end comes up automatically and only exchanges the pseudowire (VC) label.

Checkin:

Commands:

show router tunnel-table

show router isis database

show router isis database "LSP-ID" detail

tools dump router segment-routing tunnel


<img width="583" height="322" alt="image" src="https://github.com/user-attachments/assets/b404ac95-7210-4365-b967-defbeb4d5142" />

<img width="568" height="345" alt="image" src="https://github.com/user-attachments/assets/f60e4219-71ae-4448-b0da-e06fc4cc7dd1" />

<img width="571" height="635" alt="image" src="https://github.com/user-attachments/assets/5b62f0e1-e1e0-425d-ad58-9b7aea6a8d9b" />

<img width="711" height="481" alt="image" src="https://github.com/user-attachments/assets/e9f98220-28f4-4407-a774-2eaf52a0296c" />

