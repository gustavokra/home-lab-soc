# Incident 003 — Suspected Trojaned Binary (False Positive)

## 1. Resumo

O Wazuh identificou uma possível versão trojanizada do arquivo `/usr/bin/md5sum`.

- Regra: 510
- Severidade: Level 7
- Módulo: Rootcheck
- Arquivo: `/usr/bin/md5sum`

## 2. Investigação

Foi realizada uma análise do arquivo suspeito.

### Identificação do arquivo

```bash
ls -l /usr/bin/md5sum
readlink -f /usr/bin/md5sum
