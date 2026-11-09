# Incident 001 — Falha de autenticação sudo

## 1. Resumo
Simulação controlada de uma tentativa de autenticação malsucedida utilizando sudo no Ubuntu.

## 2. Ambiente
- SIEM: Wazuh 4.14.10
- Sistema monitorado: Ubuntu Linux
- Agente: Wazuh Agent 4.14.10

## 3. Detalhes do alerta
- Regra: 5503
- Descrição: PAM: User login failed.
- Nível: 5
- Usuário: gustavo
- Técnica MITRE ATT&CK: T1110.001 — Password Guessing

## 4. Análise
Uma tentativa de autenticação com senha incorreta foi realizada intencionalmente durante o laboratório.

O Wazuh coletou o evento e gerou um alerta de falha de autenticação.

## 5. Conclusão
O evento foi causado por uma simulação controlada. Não há evidências de comprometimento do sistema.

## 6. Resultado
- Coleta de logs: bem-sucedida.
- Detecção pelo Wazuh: bem-sucedida.
- Classificação MITRE ATT&CK: T1110.001.
