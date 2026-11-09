# Incident 002 — Multiple Failed Login Attempts

## 1. Resumo

Durante uma simulação controlada no Ubuntu, foram realizadas múltiplas tentativas de autenticação malsucedidas utilizando o comando `sudo`.

O Wazuh identificou eventos relacionados às falhas de autenticação, permitindo observar o comportamento do sistema diante de tentativas repetidas de senha incorreta.

## 2. Ambiente

- SIEM: Wazuh 4.14.10
- Sistema operacional: Ubuntu
- Agente monitorado: Ubuntu local
- Usuário envolvido: `gustavo`
- Serviço: sudo
- Mecanismo de autenticação: PAM

## 3. Evidências

Foram identificadas quatro ocorrências de falhas de autenticação no Wazuh.

### Regra 5503

- Descrição: PAM: User login failed.
- Severidade: 5
- Significado: falha de autenticação identificada pelo PAM.

### Regra 5557

- Descrição: unix_chkpwd: Password check failed.
- Significado: falha na verificação da senha do usuário.

## 4. Análise

As ocorrências estavam associadas ao mesmo usuário (`gustavo`) e foram produzidas intencionalmente durante uma simulação local.

A presença de múltiplas falhas em um curto intervalo pode ser um indicador de tentativa de adivinhação de senha.

Entretanto, como os eventos foram gerados em ambiente controlado, não há evidência de atividade maliciosa ou comprometimento.

Também é importante considerar que as regras 5503 e 5557 podem registrar etapas diferentes de uma mesma tentativa de autenticação.

## 5. Conclusão

O exercício demonstrou a capacidade do Wazuh de identificar e registrar falhas repetidas de autenticação por meio de eventos do PAM.

A investigação reforçou a importância de analisar a frequência, o usuário envolvido e o contexto dos eventos antes de classificar uma atividade como maliciosa.

## 6. Resultado

- [x] Simulação controlada realizada.
- [x] Eventos identificados no Wazuh.
- [x] Regras 5503 e 5557 identificadas.
- [x] Usuário envolvido identificado.
- [x] Análise e conclusão documentadas.
