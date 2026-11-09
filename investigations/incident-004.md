# Incident 003 — Possible Kernel-Level Rootkit Detection

## 1. Resumo

Em 08/11/2026, às 23:44 (horário local), o Wazuh identificou um possível processo oculto no diretório `/proc` do endpoint Ubuntu monitorado.

O alerta foi gerado pela regra `521`, com nível de severidade 11, associado à técnica MITRE ATT&CK `T1014 — Rootkit`.

O processo identificado possuía o PID `16623`.

Durante a investigação, o processo não foi encontrado no sistema. Não foram obtidas evidências suficientes para confirmar a presença de um rootkit.

## 2. Informações do Alerta

| Campo | Valor |
|---|---|
| SIEM | Wazuh |
| Endpoint | Ubuntu Linux |
| Agent ID | 001 |
| Agent IP | 192.168.100.13 |
| Rule ID | 521 |
| Severity | 11 |
| PID | 16623 |
| Decoder | rootcheck |
| MITRE ATT&CK | T1014 |
| Tactic | Defense Evasion |
| Technique | Rootkit |
| Timestamp | 08/11/2026 23:44:21 |

### Mensagem original

`Process '16623' hidden from /proc. Possible kernel level rootkit.`

## 3. Investigação

### 3.1 Verificação do processo

Foi executado:

```bash
ps -p 16623 -o pid,ppid,user,stat,comm,args
```

Resultado: nenhum processo encontrado.

Em seguida:

```bash
ls -ld /proc/16623
```

Resultado:

```text
No such file or directory
```

O processo não estava mais presente no momento da investigação.

### 3.2 Verificação dos registros do agente

Foi consultado:

```bash
sudo grep -iE 'rootcheck|rootkit|hidden process' /var/ossec/logs/ossec.log | tail -30
```

Foram encontrados registros de início e término de uma verificação Rootcheck em 09/11/2026.

Esses registros correspondem a uma verificação posterior e não comprovam a origem do alerta investigado.

### 3.3 Verificação da configuração Rootcheck

Foi consultado:

```bash
sudo grep -n -A 15 -B 5 'rootcheck' /var/ossec/etc/ossec.conf
```

A configuração demonstrou que:

- Rootcheck está habilitado.
- `check_pids` está habilitado.
- A frequência configurada é de 43200 segundos (12 horas).
- As verificações de arquivos, trojans e portas também estão habilitadas.

### 3.4 Recuperação do alerta original

O alerta foi localizado no Manager:

`/var/ossec/logs/alerts/alerts.json`

A consulta confirmou:

- Regra 521.
- Nível 11.
- PID 16623.
- Decoder `rootcheck`.
- Técnica MITRE T1014.
- Uma ocorrência registrada.

### 3.5 Verificação dos arquivos de eventos arquivados

Foi consultado:

```bash
docker compose exec wazuh.manager grep -iE '16623|rootcheck|hidden' /var/ossec/logs/archives/archives.log | tail -20
```

Nenhum registro correspondente foi encontrado.

Isso não comprova ausência de atividade suspeita, apenas indica que não foram encontrados registros adicionais nessa fonte.

## 4. Análise Técnica

O Rootcheck identificou um processo aparentemente oculto em `/proc`.

Esse comportamento pode estar associado a rootkits que utilizam mecanismos de ocultação em nível de kernel.

Entretanto, a investigação não encontrou o processo posteriormente.

Uma possível explicação é que o processo tenha terminado entre a detecção e a investigação.

Outras possibilidades incluem inconsistência temporária durante a verificação ou comportamento malicioso.

Não existem evidências suficientes para determinar qual hipótese ocorreu.

## 5. MITRE ATT&CK

| Campo | Classificação |
|---|---|
| Tática | Defense Evasion |
| Técnica | T1014 — Rootkit |

A técnica T1014 descreve o uso de rootkits para ocultar atividades maliciosas e dificultar a detecção por mecanismos de segurança.

A associação MITRE no alerta representa a classificação da regra, não a confirmação de que a técnica foi efetivamente utilizada.

## 6. Conclusão

A detecção realizada pelo Wazuh foi confirmada nos registros do Manager.

Entretanto, não foi possível confirmar a presença de um rootkit no endpoint.

O processo identificado pelo PID 16623 não estava mais disponível durante a investigação.

Não foram encontradas evidências independentes suficientes para classificar o evento como comprometimento confirmado ou falso positivo.

**Classificação final: Inconclusivo — possível falso positivo ou condição temporária.**

## 7. Recomendações

- Manter o Rootcheck habilitado.
- Monitorar novas ocorrências da regra 521.
- Investigar imediatamente novos PIDs identificados.
- Verificar processos, módulos de kernel e integridade dos arquivos caso o alerta se repita.
- Preservar os registros originais para futuras análises.

## 8. Aprendizados

Durante esta investigação foram praticadas as seguintes atividades:

- Análise de alertas de alta severidade no Wazuh.
- Investigação de processos Linux utilizando `/proc` e `ps`.
- Consulta aos logs do agente e do Manager.
- Análise da configuração Rootcheck.
- Recuperação de alertas em JSON.
- Mapeamento de eventos para MITRE ATT&CK.
- Classificação de incidentes com base em evidências.
