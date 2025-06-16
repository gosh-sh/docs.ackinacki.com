# Setting up Block Keeper Node

## **System Requirements**

<table><thead><tr><th width="155">Component</th><th width="446.31640625">Requirements</th></tr></thead><tbody><tr><td>CPU</td><td>16 dedicated physical cores on a single CPU, or 16 vCPUs ≥ 2.4 GHz. Hyper-threading must be disabled.</td></tr><tr><td>RAM</td><td>128 GB</td></tr><tr><td>Storage</td><td>1 TB of high performance NVMe SSD (PCIe Gen3 with 4 lanes or better) </td></tr><tr><td>Network</td><td>The effective bandwidth may be limited to 1 Gbps full-duplex total traffic, meaning no more than 1 Gbps in each direction (ingress and egress). A 2.5 Gbps or better full-duplex network interface card (NIC) should be installed to support anticipated future load increases and avoid hardware replacement.</td></tr></tbody></table>

## How to run

Follow the instructions in [ackinacki repo.](https://github.com/ackinacki/ackinacki)

## API access

After the Block Keeper is successfully provisioned  the gql endpoint will be on `localhost:3000/graphql`
