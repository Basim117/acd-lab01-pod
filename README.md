# ACD Lab 01 – Online Pod (student guide)

This Codespace is a small copy of your lab pod: an isolated 10.10.10.0/24 network with no internet route. Follow your **Lab 01 handout** as normal, with the changes listed below.

| IP | Host | Online version |
| --- | --- | --- |
| 10.10.10.1 | fw01 | The Codespace itself stands in for the gateway. Discover it, do not port-scan it |
| 10.10.10.10 | Kali-Analyst | Your analyst machine. You work from here |
| 10.10.10.20 | web01 | Ubuntu 22.04 web server (Apache, SSH) |
| 10.10.10.30 | win01 | Linux stand-in for the Windows client: Windows file sharing (SMB) only, ignores ping like Windows Firewall |
| 10.10.10.40 | legacy01 | Metasploitable 2 training target (deliberately vulnerable) |

## Start

1. Open the Codespace. The first start builds the pod and takes several minutes. Wait until the terminal shows the pod has started.
2. In the terminal, check that all four hosts are running:
   ```bash
   ./pod status
   ```
3. Open a shell on Kali-Analyst. Do the whole lab from this shell:
   ```bash
   ./pod kali
   ```
   Type `exit` to leave Kali. Run `./pod kali` again to go back.

## Changes from the handout

**Section 3.1 – Power on and snapshot.** There are no VMs to snapshot. For **E00**, screenshot the output of `./pod status`. If something breaks, run `./pod reset` in the Codespace terminal (not inside Kali). It rebuilds every host from a clean image, like reverting a snapshot. Your evidence is kept.

**Section 3.2 – Kali's network settings.** Skip it. Kali's address is fixed at 10.10.10.10/24.

**Section 3.3 – Evidence folder.** Run the commands as written, inside Kali. Your files also appear in the `evidence` folder in the Codespace file explorer. To download them, right-click the folder and choose **Download**.

**Task 1 – Expected results online.** Use this table instead of the one in the handout:

| Check | Expected online |
| --- | --- |
| Kali interface address | 10.10.10.10/24, state UP |
| Default route | **None.** `ip route` shows only `10.10.10.0/24` |
| Ping 10.10.10.1 | Replies |
| Ping 1.1.1.1 | `connect: Network is unreachable` |
| curl to example.com | `Could not resolve host` |
| traceroute to 1.1.1.1 | `connect: Network is unreachable` |

Answer Question 1 for the online pod: Kali has **no** default route here. How does that isolate the pod, and how is it different from the real pod, where fw01 is the gateway?

**Task 2 – Discovery.** Commands are unchanged. Hostnames appear as `web01.acdpod_podlan` and similar. MAC vendors show as `Unknown` because the network cards are virtual; record them as "virtual (locally administered)".

**Task 3 – Ports, services and OS.** Commands are unchanged. Expect three differences:

- win01 runs Windows file sharing (SMB, ports 139 and 445) on Linux. It has no Remote Desktop (RDP).
- OS detection is unreliable here. All hosts share the Codespace's Linux kernel, so expect `No exact OS matches` or a generic Linux guess. Record what Nmap says, then use service banners (for example, "Ubuntu" in the SSH version) to support your answer.
- legacy01 shows fewer services than the full Metasploitable VM, because a few of its services cannot run in a container.

**Task 4 – Asset register and baseline.** Unchanged.

## Rules

- Scan only 10.10.10.0/24, from Kali. Port-scan only web01, win01 and legacy01.
- legacy01 is deliberately vulnerable. Do not exploit it in this lab.
- When you finish, stop your Codespace: click **Codespaces** at the bottom left and choose **Stop Current Codespace**, so you do not use up your free hours.

## Pod commands

Run these in the Codespace terminal, not inside Kali.

| Command | What it does |
| --- | --- |
| `./pod status` | Show which hosts are running |
| `./pod kali` | Open a shell on Kali-Analyst |
| `./pod reset` | Rebuild all hosts from clean images; evidence is kept |
| `./pod up` | Build and start the pod (runs automatically) |
| `./pod down` | Stop and remove the pod |
