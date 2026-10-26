# Building a FortiGate Firewall Lab in EVE-NG with VMware Workstation

Hey everyone! As someone passionate about cybersecurity and networking, I wanted to get hands-on experience with enterprise firewalls. 

This project documents my step-by-step process of installing **EVE-NG** inside **VMware Workstation**, deploying a **FortiGate VM64-KVM** firewall, and configuring a dual-zone network topology with internal endpoints.

---

## What I Built

I created a virtual lab environment simulating a perimeter firewall setup connecting an internal network to the internet:

* **FortiGate VM (FortiOS)**: The core perimeter firewall filtering traffic between internal hosts and the WAN.
* **WAN Interface (`port1`)**: Connected to the external `Net` cloud for internet access.
* **LAN Interface (`port2`)**: Connected to an internal Cisco switch (`R2`).
* **Internal Clients (`VPC3`, `VPC4`)**: Virtual PCs connected to the switch on ports `e0/1` and `e0/2`.

![Network Topology](./files/topology.png)

---

## Lab Setup: VMware Workstation & EVE-NG Installation

Here is how I got the lab environment running from scratch:

1. **Importing EVE-NG into VMware**:
   * Downloaded the official EVE-NG Community OVF template.
   * Imported the OVF into VMware Workstation Pro/Player.
   * Configured the VM resource settings: **8 GB RAM**, **4 vCPUs**, and enabled **Intel VT-x/AMD-V (Virtualization Interlock)** so EVE-NG can run nested virtual machines.
   * Set the network adapter to **NAT** (or Bridged) to give EVE-NG local IP network access.

2. **FortiGate Image Deployment**:
   * Downloaded the FortiGate KVM image (`fortinet-v6.x` / `v7.x`).
   * Uploaded the uncompressed image folder to EVE-NG's QEMU directory using WinSCP (`/opt/unetlab/addons/qemu/`).
   * Fixed file permissions via SSH in EVE-NG:
     ```bash
     /opt/unetlab/wrappers/unl_wrapper -a fixpermissions
     ```

3. **Accessing the EVE-NG Web Console**:
   * Opened a browser on my main computer and entered the IP address displayed on the EVE-NG VMware screen.
   * Logged in using the default credentials (`admin` / `eve`).

---

## Step-by-Step Configuration

### 1. Building the Topology in EVE-NG
* Added the **FortiGate VM** node, a **Cisco Switch** (`R2`), two **Virtual PCs** (`VPC3` and `VPC4`), and a **Management/Net Cloud**.
* Connected FortiGate `port1` to the `Net` cloud and `port2` to switch interface `e0/0`.
* Connected `VPC3` and `VPC4` to switch interfaces `e0/1` and `e0/2`.

### 2. FortiGate Interface & GUI Access
* Booted up the FortiGate node and opened its CLI console to check the default IP address.
* Accessed the FortiGate Web Management GUI at `https://192.168.31.55`.
* Navigated to **Network > Interfaces** to edit `port2` (LAN interface):
  * Set **Alias** to `LAN`.
  * Configured administrative access privileges (**HTTPS** and **SSH** enabled).

---

## Key Learnings & Takeaways

* **Nested Virtualization**: Understood how VMware runs EVE-NG, which in turn runs FortiGate QEMU/KVM nodes underneath.
* **Firewall Interface Management**: Hands-on experience configuring physical interfaces, aliases, and management access policies inside FortiOS.
* **Troubleshooting**: Practiced fixing permission scripts in Linux and verifying IP connectivity across virtual switches and PCs.

---

## How to Run This Lab

1. Ensure VMware Workstation and EVE-NG are set up on your machine.
2. Import the FortiGate VM image into your EVE-NG QEMU folder.
3. Download/import the lab topology file or recreate the nodes following the connection layout above.
4. Start the nodes and access the FortiGate Web GUI from your browser!
