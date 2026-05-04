# Forzy — Viabilidade de um Sensor Inteligente para Monitoramento Inicial de Motor

## 1) Caso analisado

**Motor escolhido:** motor de indução trifásico 220 V, 5 cv, 60 Hz, utilizado em conjunto motobomba industrial de operação contínua.

**3 sinais monitorados (etapa inicial):**
1. **Corrente (A)**
2. **Temperatura da carcaça/mancal (°C)**
3. **Vibração RMS (mm/s)**

---

## 2) Arquitetura inicial proposta (Cenário A como base)

### Diagrama simples

```text
[Motor 220 V]
     │
     ├─ Sensor de corrente (TC)
     ├─ Sensor de temperatura (PT100/NTC)
     └─ Sensor de vibração (acelerômetro)
           │
           ▼
 [Módulo de aquisição / Edge gateway]
           │
      (OPC-UA / IoT)
           │
           ▼
 [Processamento inicial no servidor]
 (validação, agregação, alarmes básicos)
           │
           ▼
 [Armazenamento histórico]
 (banco de séries temporais + dashboard)
```

---

## 3) Análise de viabilidade

### Por que esses sinais são relevantes
- **Corrente:** indica carga do motor, desequilíbrios e possíveis condições de sobrecorrente/partida anormal.
- **Temperatura:** ajuda a detectar sobreaquecimento por falha de ventilação, sobrecarga ou degradação de rolamento.
- **Vibração:** é um dos melhores indicadores precoces de desalinhamento, desbalanceamento e desgaste mecânico.

### Onde a arquitetura clássica pode gerar custo/gargalo
- Na arquitetura clássica, a maior parte dos dados brutos é enviada continuamente ao servidor.
- Isso aumenta **tráfego de rede**, **escrita em banco** e **consumo energético computacional** no pipeline de ingestão/armazenamento.
- A separação rígida entre memória e processamento tende a ampliar movimentação de dados, especialmente para vibração com taxa de amostragem mais alta.

### Faz sentido pensar em sensor inteligente no futuro?
- Sim, principalmente para vibração e eventos transitórios.
- Um sensor inteligente poderia realizar **pré-processamento local por eventos** (ex.: extração de RMS, curtose, envelope e detecção de anomalia simples), enviando ao servidor apenas dados relevantes.
- Benefícios esperados: menor latência para alarme, menor volume de dados e melhor escalabilidade para múltiplos motores.

### Memristores como possibilidade futura
- É tecnicamente válido citar memristores como hipótese de evolução para memória local não volátil e chaveamento resistivo em arquiteturas neuromórficas.
- Neste sprint, a citação deve ficar em nível conceitual/roadmap tecnológico, sem compromisso de implementação imediata.

---

## 4) Recomendação final

**Recomendação escolhida: _Cenário B_**

Começar com a **arquitetura clássica** (baixo risco, integração rápida com OPC-UA/IoT e geração de histórico), **registrando desde já uma trilha de evolução** para sensor inteligente de borda com pré-processamento por eventos.

Essa decisão equilibra entrega rápida no curto prazo e preparação para reduzir gargalos de dados/energia no médio prazo.

---

## 5) Backlog mínimo (4 próximos passos)

1. **Definir instrumentação mínima** do piloto (modelos de sensores, faixa de medição e taxa de amostragem por sinal).
2. **Implementar pipeline clássico** de aquisição OPC-UA/IoT, armazenamento temporal e dashboard de tendência/alarmes.
3. **Estabelecer linha de base operacional** do motor (janela de 2 a 4 semanas) para limites iniciais de corrente, temperatura e vibração.
4. **Prototipar estudo de edge intelligence** (filtro por eventos + features locais), incluindo nota técnica sobre possível uso futuro de memristores.
