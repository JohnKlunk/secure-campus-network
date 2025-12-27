# VLAN Design

The network uses the following VLANs:

| VLAN ID | Name     | Purpose |
|-------|----------|--------|
| 10 | ADMIN | Administrative systems |
| 20 | TEACHER | Teacher workstations |
| 30 | STUDENT | Student devices (restricted) |
| 40 | SERVER | Infrastructure servers |

Each VLAN uses its own /24 subnet and default gateway on the core router.

Inter-VLAN communication is controlled using Access Control Lists (ACLs).
