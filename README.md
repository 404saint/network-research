# Network Research Labs

![Wireshark](https://img.shields.io/badge/Wireshark-000000?style=for-the-badge&logo=wireshark&logoColor=00FFFF)
![Kali Linux](https://img.shields.io/badge/Kali-Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Graphviz](https://img.shields.io/badge/Graphviz-E34F26?style=for-the-badge&logo=graphviz&logoColor=white)

A collection of network research experiments and lab setups by **RUGERO Tesla (404saint)**.  
Focused on exploring constrained network environments, routing pivots, and application-layer traffic analysis.

---

## 🔹 Featured Labs

| Lab Name | Description |
|----------|-------------|
| ADB Userspace VPN Pivot | Reverse tethering using ADB and SOCKS5h to route PC traffic through a mobile VPN. |
| *(Future labs)* | Additional network experiments exploring constrained routing, traffic pivots, and application-layer analysis. |

---

## Repository Structure (for Labs)

```

network-research/
│
├─ lab-setup/               # Environment setups and reproducibility notes
├─ methodology/             # Problem analysis, design decisions, implementation, technical breakdown
├─ security-considerations/ # Defensive and ICS/OT perspective
├─ references/              # Technical notes, RFCs, documentation
└─ diagrams/                # Architecture diagrams and flowcharts

```

---

## ⚙️ Tools & Techniques

- Linux/Windows hosts for testing  
- Network proxies, port forwarding, and tunneling techniques  
- Application-layer pivoting and constrained routing  
- Packet inspection, traffic analysis, and validation tools  
- Diagramming with Graphviz/DOT  

---

## 📌 Notes

- Labs are **reversible and safe**, designed for research purposes.  
- Focus is on **understanding network behavior under constraints**, not bypassing security controls.  
- Suitable for ICS/OT-inspired research and general network experimentation.

---

## 🛠️ Contributing / Adding New Labs

This repository is designed to grow with new network research experiments. To add a lab:

1. **Create a dedicated directory** for the lab:
```

network-research/<lab-name>/

```
2. Include the following structure:
```

lab-setup/               # Environment setup & prerequisites
methodology/             # Problem analysis, design, implementation
security-considerations/ # Defensive insights & ICS/OT perspective
references/              # Documentation, RFCs, technical notes
diagrams/                # Architecture or flow diagrams

```
3. Include a brief description in the **README.md** of the lab folder.  
4. Use clear naming for files and directories to maintain consistency.  
5. Ensure all experiments are **reversible, safe, and for research purposes only**.

> Contributions should follow these guidelines to maintain clarity, reproducibility, and research integrity.

---

## Contact

- GitHub: [404saint](https://github.com/404saint)  
- HackerOne: [404saint](https://hackerone.com/404saint)  
- Website: [404saint](https://saint404.lovable.app/)  
- Email: *rugerotesla@proton.me*

---

*Industrial & Network Security | Offensive Security Research | ICS/OT-focused*
