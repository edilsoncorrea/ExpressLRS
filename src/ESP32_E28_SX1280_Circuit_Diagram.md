# Diagrama de Circuito ESP32 + E28 LoRa SX1280 - TX DYU

## 📐 Diagrama Esquemático Completo

### 🔌 Conexões Principais

```
                    ┌─────────────────────────────────────┐
                    │           ESP32 DevKit              │
                    │                                     │
    ┌───────────────┤ 3.3V                         GPIO23├─────────┐
    │               │                                     │         │
    │   ┌───────────┤ GND                          GPIO19├─────┐   │
    │   │           │                                     │     │   │
    │   │       ┌───┤ GPIO18                       GPIO5 ├───┐ │   │
    │   │       │   │                                     │   │ │   │
    │   │       │ ┌─┤ GPIO14                       GPIO4 ├─┐ │ │   │
    │   │       │ │ │                                     │ │ │ │   │
    │   │       │ │ │ GPIO21                      GPIO27 ├─│─│─│─┐ │
    │   │       │ │ │                                     │ │ │ │ │ │
    │   │       │ │ │ GPIO22                      GPIO26 ├─│─│─│─│─│─┐
    │   │       │ │ │                                     │ │ │ │ │ │ │
    │   │       │ │ │ GPIO35                             │ │ │ │ │ │ │
    │   │       │ │ │                                     │ │ │ │ │ │ │
    │   │       │ │ │ GPIO15                             │ │ │ │ │ │ │
    │   │       │ │ │                                     │ │ │ │ │ │ │
    │   │       │ │ └─────────────────────────────────────┘ │ │ │ │ │ │
    │   │       │ │                                         │ │ │ │ │ │
    │   │       │ │              E28-2G4M27S               │ │ │ │ │ │
    │   │       │ │        ┌─────────────────────┐         │ │ │ │ │ │
    │   │       │ └────────┤ RST                 │         │ │ │ │ │ │
    │   │       │          │                     │         │ │ │ │ │ │
    │   │       └──────────┤ SCK           BUSY ├─────────┘ │ │ │ │ │
    │   │                  │                     │           │ │ │ │ │
    │   └──────────────────┤ GND           DIO1 ├───────────┘ │ │ │ │
    │                      │                     │             │ │ │ │
    │              ┌───────┤ VCC            NSS ├─────────────┘ │ │ │
    │              │       │                     │               │ │ │
    └──────────────│───────┤ 3.3V          MISO ├───────────────┘ │ │
                   │       │                     │                 │ │
                   │   ┌───┤ NC            MOSI ├─────────────────┘ │
                   │   │   │                     │                   │
                   │   │   │ NC            RXEN ├───────────────────│─┘
                   │   │   │                     │                   │
                   │   │   │ NC            TXEN ├───────────────────┘
                   │   │   │                     │
                   │   │   │ ANT                 │     ┌─────────────┐
                   │   │   │  │                  │     │   Antena    │
                   │   │   │  └──────────────────│─────┤   2.4GHz    │
                   │   │   │                     │     │             │
                   │   │   └─────────────────────┘     └─────────────┘
                   │   │
                   │   │      Display OLED I2C
                   │   │    ┌─────────────────────┐
                   │   └────┤ GND                 │
                   │        │                     │
                   └────────┤ VCC           SDA ├─────── GPIO21
                            │                     │
                    ────────┤ SCL           SCL ├─────── GPIO22
                            │                     │
                            └─────────────────────┘

                              Joystick 5-Way
                         ┌─────────────────────────┐
                         │                         │
                    UP───┤R1├──┐                   │
                         │     │                   │
                  DOWN───┤R2├──┤                   │
                         │     │                   │
                  LEFT───┤R3├──┼───────────── GPIO35
                         │     │                   │
                 RIGHT───┤R4├──┤                   │
                         │     │                   │
                CENTER───┤R5├──┘                   │
                         │     │                   │
                         │   10kΩ                  │
                         │     │                   │
                         │   3.3V                  │
                         │                         │
                         │   GND ──────────────────┘
                         │                         
                         └─────────────────────────┘
```

## 📋 Especificações dos Componentes

### E28-2G4M27S (SX1280)
```
Chip:           SX1280
Frequência:     2400-2500 MHz
Potência:       +27dBm (500mW) máxima
Modulação:      LoRa, FLRC, GFSK
Interface:      SPI
Tensão:         3.3V
Antena:         IPEX/U.FL connector
```

### Resistores para Joystick (Valores Sugeridos)
```
R1 (UP):        470Ω  → ADC ~1905
R2 (DOWN):      820Ω  → ADC ~1160
R3 (LEFT):      1.2kΩ → ADC ~580
R4 (RIGHT):     390Ω  → ADC ~2580
R5 (CENTER):    10kΩ  → ADC ~0
Pull-up:        10kΩ  → ADC ~4095 (idle)
```

## 🔧 Detalhes das Conexões

### SPI Bus (Principais)
```
ESP32    E28      Função                Observações
-----    ---      ------                -----------
GPIO18   SCK      SPI Clock             Máx 10MHz
GPIO23   MOSI     Master Out Slave In   Dados ESP32→E28
GPIO19   MISO     Master In Slave Out   Dados E28→ESP32
GPIO5    NSS      Slave Select          Chip Select (ativo baixo)
```

### Controle e Status
```
ESP32    E28      Função                Observações
-----    ---      ------                -----------
GPIO14   RST      Reset                 Reset ativo baixo
GPIO4    DIO1     Digital I/O 1         Interrupt para ESP32
GPIO21   BUSY     Busy Signal           Status ocupado (ativo alto)
GPIO27   RXEN     RX Enable             Controle RX (ativo alto)
GPIO26   TXEN     TX Enable             Controle TX (ativo alto)
```

### Alimentação
```
ESP32    E28      Especificação
-----    ---      -------------
3.3V     VCC      Tensão de alimentação 3.3V ±5%
GND      GND      Terra comum
                  Consumo: ~120mA (TX), ~12mA (RX)
```

## 📡 Layout da PCB (Sugestões)

### Considerações de RF
```
1. Manter trilhas SPI curtas (<5cm)
2. Plano de terra sólido sob o E28
3. Antena longe de componentes digitais
4. Filtros de alimentação (LC ou ferrite beads)
5. Bypass capacitors próximos ao E28 (100nF, 10µF)
```

### Filtros Recomendados
```
Alimentação 3.3V:
├── L1: 10µH (Ferrite bead)
├── C1: 10µF (Tantalum)
├── C2: 100nF (Cerâmico)
└── C3: 10pF (Cerâmico)

Líneas SPI:
├── Series resistors: 33Ω em SCK, MOSI
└── Pull-up em NSS: 10kΩ
```

## ⚡ Levels de Tensão

### ESP32 GPIOs
```
VIL (Low):  0V - 0.8V
VIH (High): 2.0V - 3.3V
VOL (Low):  0V - 0.4V  
VOH (High): 2.8V - 3.3V
Corrente:   ±40mA máx por pino
```

### SX1280 (E28)
```
VIL (Low):  0V - 0.6V
VIH (High): 1.8V - 3.3V
VOL (Low):  0V - 0.4V
VOH (High): 2.4V - 3.3V
Compatível: ✅ Direto com ESP32
```

## 🛡️ Proteções Recomendadas

### ESD Protection
```
- TVS diodes nas linhas de antena
- ESD protection nos pinos expostos
- Chassis grounding adequado
```

### RF Shielding
```
- Shield metálico sobre E28 (opcional)
- Compartimento RF separado
- Filtros de EMI na alimentação
```

## 📏 Dimensões Físicas

### E28-2G4M27S
```
Tamanho:    16mm × 26mm × 3mm
Pads:       20 pinos (0.5mm pitch)
Antena:     Conector IPEX
Mounting:   SMD (surface mount)
```

### Clearances Mínimas
```
E28 ↔ Metal:     ≥3mm
E28 ↔ Crystal:   ≥5mm  
E28 ↔ Switching: ≥5mm
Antena ↔ PCB:    ≥10mm
```

## 🔍 Testing Points

### Debug/Test Pads
```
TP1: 3.3V Rail
TP2: GND
TP3: SPI_SCK
TP4: SPI_MOSI  
TP5: SPI_MISO
TP6: DIO1
TP7: BUSY
TP8: RF_OUT (após filtro)
```

---

**Importante:** Este é um diagrama de referência baseado na configuração do ExpressLRS. Para implementação em PCB, consulte também:
- Datasheet oficial do SX1280
- Reference design da Semtech  
- Guias de layout RF
- Certificações regulatórias locais

**Data:** 05 de Novembro de 2025  
**Fonte:** ExpressLRS DIY ESP32 E28 Configuration