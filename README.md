# 🛡️ Forensic Log Analyzer
 
> A Bash automation tool for the **initial triage** of web server access logs during incident response — turning raw `access_log` files into structured evidence, threat intelligence and an executive report.
 
---
 
## 🕵️ Overview
 
Forensic Log Analyzer parses raw web server logs (Apache / Nginx **Combined Log Format**, plain or gzipped) and automatically surfaces known attack signatures (OWASP Top 10), scanner activity and traffic anomalies. Results are written to a timestamped, chain-of-custody-friendly directory tree so they can be attached directly to an incident ticket.
 
It is a **read-only, defensive (Blue Team)** tool: it never acts against any host and never modifies the system under analysis. Blocking suggestions are *generated*, never applied.
 
## ✨ Key features
 
- **URL-decoding before analysis** — catches encoded payloads (`%27`, `%3Cscript%3E`) that naive `grep`-based scripts miss.
- **Context-aware detection** — regex is applied to the decoded request field only, which keeps false positives low. RCE detection, for example, requires a shell metacharacter (`;`, `|`, `` ` ``, `$()`) before a command, so a plain `id` inside a URL is not flagged.
- **Attack vectors:** SQL Injection, XSS, LFI, Path Traversal and RCE / Command Injection.
- **Scanner & tool fingerprinting:** Nmap, SQLMap, Nikto, Metasploit, Burp, ffuf, gobuster, wpscan, hydra and more.
- **Traffic anomalies:** high-volume IPs (adaptive threshold), dangerous HTTP methods (PUT/DELETE/TRACE/CONNECT) and **authentication brute-force** (bursts of `401`/`403` per IP).
- **Threat intelligence:** consolidated malicious IP list, a ready-to-review `ip_blocklist.txt` and suggested (not executed) `iptables` rules.
- **Chain of custody:** SHA-256 of the source log is recorded *before* any processing, and every generated artifact is hashed into a manifest.
- **Clean reports:** the terminal output is colorized, but the report file is plain text (no ANSI noise).
## 📦 Requirements
 
- `bash`, GNU coreutils, `awk`, `grep`, `tar` (standard on any Linux)
- `zcat` — only if you analyze `.gz` logs
- `perl` **or** `python3` — optional, used for faster URL-decoding (a pure-Bash fallback is used otherwise)
## 🚀 Usage
 
```bash
chmod +x forensic_log_analyzer.sh
 
# Basic
./forensic_log_analyzer.sh /var/log/nginx/access.log
 
# Custom output directory
./forensic_log_analyzer.sh /var/log/apache2/access.log.gz ./cases/incident-042
```
 
```text
Usage: ./forensic_log_analyzer.sh <log_file> [output_dir]
 
  <log_file>    Access log (plain text or .gz), Combined Log Format
  [output_dir]  Optional. Default: ./forensic_analysis_<timestamp>
```
 
## 📂 Output structure
 
```text
📦 forensic_analysis_YYYYMMDD_HHMMSS
 ┣ 📂 evidence
 ┃ ┣ 📜 sqli_ips.txt             # IPs with SQL Injection attempts
 ┃ ┣ 📜 xss_ips.txt              # IPs with XSS attempts
 ┃ ┣ 📜 lfi_ips.txt              # IPs with LFI / sensitive-file access
 ┃ ┣ 📜 traversal_ips.txt        # IPs with path traversal attempts
 ┃ ┣ 📜 rce_ips.txt              # IPs with command-injection attempts
 ┃ ┣ 📜 scanner_ips.txt          # IPs using known scanners
 ┃ ┣ 📜 auth_bruteforce_ips.txt  # IPs with 401/403 bursts
 ┃ ┗ 📜 high_volume_ips.txt      # Abnormal request volume
 ┣ 📂 statistics
 ┃ ┣ 📜 top_ips.txt
 ┃ ┣ 📜 http_methods.txt
 ┃ ┗ 📜 status_codes.txt
 ┣ 📂 threat_intelligence
 ┃ ┣ 📜 malicious_ips.txt        # Consolidated, de-duplicated
 ┃ ┣ 📜 ip_blocklist.txt         # Review before applying
 ┃ ┣ 📜 iptables_suggested.sh    # Suggested rules — NOT executed
 ┃ ┗ 📜 iocs.txt                 # Indicators of Compromise
 ┣ 📜 security_analysis_report.txt   # Executive report (plain text)
 ┗ 📜 MANIFEST_sha256.txt            # Integrity hashes (source + artifacts)
```
 
A compressed `forensic_analysis_YYYYMMDD_HHMMSS.tar.gz` is also produced for archiving.
 
## 🧪 Example
 
```text
### DETECCAO DE AMEACAS (OWASP)
[CRITICO] SQL Injection: 1 requisicoes suspeitas, 1 IPs unicos (evidence/sqli_ips.txt)
[ALTO]    XSS: 1 requisicoes suspeitas, 1 IPs unicos (evidence/xss_ips.txt)
[CRITICO] RCE / Command Injection: 1 requisicoes suspeitas, 1 IPs unicos (evidence/rce_ips.txt)
 
### RESUMO EXECUTIVO
Niveis de ameaca: CRITICO=2 ALTO=5 MEDIO=0 BAIXO=0
```
 
## ⚠️ Responsible use
 
This tool is for **authorized** incident response and educational use on logs you own or are permitted to analyze. The author is not responsible for misuse.
 
## 📝 License
 
MIT — see `LICENSE`.
 
---
 
**Author:** Leonardo Pereira ([@Leobatman](https://github.com/Leobatman)) · Offensive Security student · OSCP candidate
 
