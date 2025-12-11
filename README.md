# 🛡️ Forensic Log Analyzer
![Platform](https://img.shields.io/badge/platform-Linux-black?logo=linux)
![Language](https://img.shields.io/badge/language-Bash%20Script-green?logo=gnu-bash)
![Focus](https://img.shields.io/badge/focus-Cybersecurity%20%7C%20Forensics-red)
<div align="center">
 <img align="center" height="10000" src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExa2g0bjZiMzE0bDJ5anptZ2tjYXVuMWFpeXZybXJuNzZnbm01cTMybCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/12W5Sg2koWYnwA/giphy.gif"  />

</div>
> Uma ferramenta profissional de automação para análise forense de logs de servidores web, projetada para identificar padrões de ataque, gerar inteligência de ameaças e criar relatórios executivos.

---

## 🕵️‍♂️ Visão Geral

[cite_start]O **Forensic Log Analyzer** é um script robusto em Bash que incorpora as melhores práticas do setor de segurança da informação. Ele automatiza a triagem inicial durante uma resposta a incidentes, processando arquivos `access_log` brutos e transformando-os em inteligência acionável.

A ferramenta realiza uma varredura profunda buscando assinaturas de ataques conhecidos (OWASP Top 10) e anomalias de tráfego, organizando os resultados em uma estrutura de diretórios forenses.

## 🚀 Funcionalidades Principais

* **🔍 Detecção de Vetores de Ataque:**
    Identifica automaticamente tentativas de:
    * SQL Injection (SQLi) [cite: 235]
    * Cross-Site Scripting (XSS) [cite: 257]
    * Local File Inclusion (LFI) & Path Traversal [cite: 275, 311]
    * Remote Code Execution (RCE) [cite: 293]
* **🤖 Detecção de Bots e Scanners:**
    Identifica ferramentas automatizadas como Nmap, SQLMap, Nikto, Metasploit, entre outros[cite: 48].
* **📊 Relatório Executivo:**
    Gera um sumário gerencial classificando os riscos por níveis de criticidade (Crítico, Alto, Médio, Baixo)[cite: 7, 8].
* **📂 Gestão de Evidências:**
    Coleta e preserva artefatos digitais, separando IPs suspeitos e logs filtrados em pastas específicas[cite: 6].
* **🛡️ Threat Intelligence:**
    Gera automaticamente listas de bloqueio (`ip_blocklist.txt`) e IOCs baseados nos ataques detectados[cite: 347, 353].

## 📂 Estrutura de Saída

[cite_start]Ao executar a análise, o script cria um diretório organizado com carimbo de data/hora[cite: 28]:

```text
📦 forensic_analysis_YYYYMMDD_HHMMSS
 ┣ 📂 evidence
 ┃ ┣ 📜 sqli_ips.txt         # IPs que tentaram SQL Injection
 ┃ ┣ 📜 xss_ips.txt          # IPs que tentaram XSS
 ┃ ┗ 📜 sensitive_access.txt # Tentativas de acesso a painéis admin
 ┣ 📂 statistics
 ┃ ┣ 📜 top_ips.txt          # Maiores ofensores por volume
 ┃ ┗ 📜 hourly_distribution.txt
 ┣ 📂 threat_intelligence
 ┃ ┣ 📜 iocs.txt             # Indicadores de Comprometimento gerados
 ┃ ┗ 📜 ip_blocklist.txt     # Lista sugerida para bloqueio no Firewall
 ┗ 📜 security_analysis_report.txt  # Relatório Executivo Final



Analista de Cibersegurança e Desenvolvedor.

Script desenvolvido seguindo metodologia forense digital formal. 

⚠️ Disclaimer: Esta ferramenta foi desenvolvida para fins educacionais e de defesa (Blue Team). O autor não se responsabiliza pelo uso indevido.
