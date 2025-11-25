README.md — Projeto IoT Garagem (ESP32 + MicroPython + MQTT)
Projeto IoT – Controle de Vagas de Garagem com ESP32, MicroPython e MQTT

Este projeto implementa uma simulação IoT de controle de vagas de garagem em condomínio, usando:

ESP32 (MicroPython) rodando no Wokwi

LEDs e botão simulados

Broker MQTT público (HiveMQ)

Cliente WebSocket para monitoramento em tempo real

Regras de autorização de vagas (simples/dupla)

Ele representa de forma fiel um sistema real onde a administração do condomínio recebe alertas imediatamente caso um carro pare na vaga errada.

 1. Objetivo do Projeto

Problema real do condomínio:
➡ Moradores com vaga simples estavam estacionando em vagas duplas (ou vice-versa).
➡ Isso gera conflitos, multas e trabalho extra para a administração.

Solução criada:
✔ Cada carro tem uma TAG / identificação
✔ Cada vaga tem regras (ex.: “303A” → simples)
✔ Um sensor lógico detecta qual carro está na vaga
✔ ESP32 determina se o carro está autorizado
✔ LEDs indicam localmente:

Verde → carro correto

Vermelho → carro incorreto

✔ Se for errado → envia alerta MQTT para a administração
✔ A administração recebe em tempo real via HiveMQ WebSocket Client

2. Arquitetura da Solução
Wokwi ESP32 → Código Python (MicroPython)
    │
    ├── LEDs (status)
    ├── Botão (troca simulação de TAG)
    │
    └── MQTT (broker.hivemq.com)
             │
             └── WebSocket Client (Admin) recebe alerta


Tecnologias utilizadas

ESP32 (simulado)

MicroPython

MQTT (protocolo de mensagens)

HiveMQ Public Broker

HiveMQ WebSocket Client

Wokwi IoT Simulator

 3. Como funciona a lógica

O sistema possui uma lista de “carros”:

CARROS = [
    {"tag": "TAG-303A", "vagas": "303A"},
    {"tag": "TAG-303B", "vagas": "303A|303B"},
    {"tag": "TAG-404B", "vagas": "404B"}
]


O botão troca qual TAG está “estacionando”.

O código verifica:

tipo da vaga

vagas permitidas pelo veículo

se a TAG corresponde à vaga

Acende LED verde ou vermelho.

Se vermelho → publica alerta no tópico MQTT:

condominio/garagem/alertas


Payload:

{
  "vaga_id": "303A",
  "tag": "TAG-404B",
  "status": "ERRADO",
  "motivo": "vaga incorreta"
}

4. Autores

Giovanna Bornéo Pereira – Desenvolvimento do script Python e integração MQTT

Lucas – Apoio no código e teste da lógica

Ryan – Testes e revisão da regra de vagas

Camilly – Documentação e apoio na montagem do circuito

5. Conclusão

Este projeto demonstra de forma simples e completa um sistema IoT real:

Dispositivo (ESP32)

Lógica embarcada

Comunicação MQTT

Monitoramento WebSocket

Integração com regras do mundo real (vagas simples/duplas)

É 100% replicável, 100% funcional e totalmente adequado para apresentar como um sistema IoT baseado em nuvem.
