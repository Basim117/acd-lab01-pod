# ACD Lab 01 – Online Pod (instructor guide)

Use this when the range is unavailable. Each student gets their own isolated copy of the Lab 01 hosts inside a GitHub Codespace, opened in a web browser. Nothing is installed on student laptops.

## Set it up (about 15 minutes, once)

1. Create a new GitHub repository (private is fine) and upload every file in this folder, including the hidden `.devcontainer` folder.
2. In the repository settings, tick **Template repository**.
3. Test it yourself before class: **Code → Codespaces → Create codespace on main**. Wait for the first build, then follow `README.md` through Task 2 to confirm the five expected hosts appear.
4. Give students the repository link. Each student clicks **Use this template → Open in a codespace**, or you hand it out as a GitHub Classroom assignment with Codespaces enabled.

Students need a free GitHub account. Delete `INSTRUCTOR.md` from the students' copy if you don't want them to see it.

## Cost

- The Codespace is set to a 2-core, 8 GB machine.
- GitHub Free accounts include 120 core hours a month, which is 60 hours on a 2-core machine. That is far more than Lab 01 needs.
- Verified GitHub students get more, and verified teachers can use GitHub Classroom's free allowance instead of students' own hours.
- Remind students to stop their Codespace when they finish. An idle Codespace stops itself after a timeout, but a running one uses hours.

## What is different from the real pod

| Real pod | Online pod | Effect on the answer key |
| --- | --- | --- |
| fw01 (10.10.10.1) is the gateway, uplink closed | The Codespace holds 10.10.10.1; the network has **no** default route | Q1: isolation comes from having no route at all, not from a gateway that won't forward |
| win01 is Windows 10/11 (SMB, RDP) | Linux stand-in running Samba (139, 445), ignores ping | Q2 unchanged: win01 is still missing from the ICMP sweep |
| Suricata on fw01 | No IDS at all | Q4: nothing watches the segment; the "placement" point becomes the main answer |
| legacy01 is the Metasploitable VM | Metasploitable 2 container | Q5 unchanged; a few services (BIND, NFS) do not run in the container |
| Nmap OS detection works per host | Containers share one Linux kernel | Expect "No exact OS matches" or generic Linux for every host |
| VM snapshots | `./pod reset` rebuilds clean containers | Evidence E00 is a screenshot of `./pod status` |

## Optional: the unannounced host

`./pod surprise` starts a small web server called `dev` at 10.10.10.60, matching the "forgotten host" idea in your pod. Students cannot see it in the README.

To have it running in every student's pod automatically, edit `.devcontainer/devcontainer.json` before students create their Codespaces:

```json
"postCreateCommand": "chmod +x pod && ./pod up && ./pod surprise",
"postStartCommand": "chmod +x pod && ./pod start && ./pod surprise",
```

Students should find it in Task 2, record it as "unauthorized / investigate", and not port-scan it.

## What has been tested

Tested end to end with Ubuntu-based stand-ins for Kali and Metasploitable:

- the isolated network, fixed addresses and no internet route
- every handout command in Tasks 1 to 4
- win01 ignoring ping while still appearing in the ARP sweep
- evidence files appearing in the `evidence` folder
- `./pod surprise`

Not tested, because the build environment could not download from Docker Hub:

- the real `kalilinux/kali-rolling` and `tleemcjr/metasploitable2` images
- the Codespaces start-up itself

Run step 3 of the setup before relying on it in class.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| `./pod status` shows a host missing | Run `./pod up`. The first build downloads about 1 GB of images and can take several minutes |
| Image download fails with a rate-limit message | Wait a few minutes and run `./pod up` again |
| A student broke a host | `./pod reset` (evidence is kept) |
| A student wants to work on their own laptop | Any machine with Docker Desktop: open this folder in a terminal and run `docker compose up -d --build`, then `docker compose exec kali bash` |

## Note on GitHub's terms

GitHub's terms say Codespaces should be used for developing or testing the software project in the repository. This lab scans only containers inside each student's own Codespace and never reaches the internet, but check that this use fits your institution's agreement with GitHub (for example, GitHub Education or GitHub Classroom).
