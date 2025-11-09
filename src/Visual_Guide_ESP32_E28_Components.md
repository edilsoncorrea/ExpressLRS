# Guia Visual DIY ESP32 DevKit + E28 LoRa SX1280 - TX DYU

## 📸 Identificação dos Componentes

### 🔧 ESP32 DevKit V1 (30 pinos)

```
Aparência Visual:
┌─────────────────────────────────────┐
│  ┌─────┐    ESP32 DEVKIT V1    ┌───┐│
│  │ USB │                       │LED││
│  │  C  │   ┌─────────────┐     │   ││
│  │     │   │    ESP32    │     │PWR││
│  └─────┘   │   WROOM-32  │     └───┘│
│             └─────────────┘          │
│ 3V3 ○○ EN                    D23 ○○ │
│ GND ○○ SVP                   D22 ○○ │
│ D15 ○○ SVN                   TXD ○○ │
│ D2  ○○ D34                   RXD ○○ │
│ D4  ○○ D35                   D21 ○○ │
│ D16 ○○ D32                   GND ○○ │
│ D17 ○○ D33                   D19 ○○ │
│ D5  ○○ D25                   D18 ○○ │
│ D18 ○○ D26                   D5  ○○ │
│ D19 ○○ D27                   D17 ○○ │
│ GND ○○ D14                   D16 ○○ │
│ D21 ○○ D12                   D4  ○○ │
│ RXD ○○ D13                   D2  ○○ │
│ TXD ○○ GND                   D15 ○○ │
│ D22 ○○ VIN                   GND ○○ │
│ D23 ○○                       3V3 ○○ │
└─────────────────────────────────────┘

Características:
✅ Chip: ESP32-D0WD-V3
✅ Flash: 4MB
✅ WiFi + Bluetooth
✅ Tensão: 3.3V/5V via USB
✅ 30 GPIOs disponíveis
✅ Conversor USB-UART: CP2102
```

### 📡 Módulo E28-2G4M27S (SX1280)

```
Aparência Visual:
┌─────────────────────────┐
│  E28-2G4M27S            │
│                         │
│  ┌─────────────────┐   ┌┴┐
│  │                 │   │A│ ← Conector IPEX (antena)
│  │    SX1280       │   │N│
│  │                 │   │T│
│  │   2.4GHz LoRa   │   └┬┘
│  └─────────────────┘    │
│                         │
│ ●●●●●●●●●●● ●●●●●●●●●●● │ ← 20 pads SMD
│ 1         10 11       20│
│                         │
│  EBYTE                  │
└─────────────────────────┘

Tamanho: 16mm × 26mm × 3mm

Pinout E28-2G4M27S:
┌─────────────────────────┐
│Pin │ Nome │ Função       │
├────┼──────┼─────────────┤
│ 1  │ GND  │ Terra        │
│ 2  │ VCC  │ 3.3V         │
│ 3  │ NC   │ Não conectar │
│ 4  │ NC   │ Não conectar │
│ 5  │ NC   │ Não conectar │
│ 6  │ NC   │ Não conectar │
│ 7  │ MISO │ SPI Data Out │
│ 8  │ MOSI │ SPI Data In  │
│ 9  │ SCK  │ SPI Clock    │
│ 10 │ NSS  │ SPI CS       │
│ 11 │ RST  │ Reset        │
│ 12 │ DIO1 │ Interrupt    │
│ 13 │ BUSY │ Status       │
│ 14 │ NC   │ Não conectar │
│ 15 │ NC   │ Não conectar │
│ 16 │ RXEN │ RX Enable    │
│ 17 │ TXEN │ TX Enable    │
│ 18 │ NC   │ Não conectar │
│ 19 │ NC   │ Não conectar │
│ 20 │ GND  │ Terra        │
└─────────────────────────┘

Características:
✅ Chip: Semtech SX1280
✅ Frequência: 2400-2500 MHz
✅ Potência: +27dBm (500mW)
✅ Alcance: 2-5km (linha de vista)
✅ Interface: SPI
✅ Modulação: LoRa, FLRC, GFSK
```

### 📺 Display OLED I2C (SSD1306)

```
Aparência Visual:
┌───────────────────────┐
│ ┌─────────────────┐   │
│ │                 │   │ ← Tela OLED 128x64
│ │   OLED DISPLAY  │   │
│ │                 │   │
│ │    128 x 64     │   │
│ └─────────────────┘   │
│                       │
│ GND VCC SCL SDA       │ ← 4 pinos
│  ○   ○   ○   ○        │
└───────────────────────┘

Tamanho típico: 27mm x 27mm

Conexões:
✅ GND → GND do ESP32
✅ VCC → 3.3V do ESP32  
✅ SCL → GPIO22 (Clock)
✅ SDA → GPIO21 (Data)
✅ Endereço I2C: 0x3C ou 0x3D
```

### 🕹️ Joystick 5-Way (Navegação)

```
Aparência Visual:
     ┌─────────┐
     │    ↑    │ ← UP
     │  ←   → │ ← LEFT/RIGHT  
     │    ↓    │ ← DOWN
     └─────────┘
         ↑
      CENTER (push)

Conexões típicas:
┌─────────────────┐
│ C  L  D  R  U   │ ← 5 terminais
│ ○  ○  ○  ○  ○   │
└─────────────────┘
C=Center, L=Left, D=Down, R=Right, U=Up

Esquema com Resistores:
UP    ─┤470Ω├─┐
DOWN  ─┤820Ω├─┤
LEFT  ─┤1.2k├─┼── GPIO35 (ADC)
RIGHT ─┤390Ω├─┤
CENTER─┤ 10k├─┘
              │
            10kΩ pullup to 3.3V
```

### 📶 Antena 2.4GHz

```
Tipos Recomendados:

1. Antena Duck (Monopole):
   ┌─┐
   │ │ ← 6-10cm altura  
   │ │   Ganho: 2-3dBi
   └─┘
    │
   IPEX

2. Antena Dipole:
   ──────●────── ← Elemento irradiante
         │       Ganho: 2-5dBi  
        IPEX     Direcional

3. Antena PCB (integrada):
   ╭─────────╮
   │ ∿∿∿∿∿∿∿ │ ← Trilha meandro
   │         │   Compacta
   ╰─────────╯
```

## 🛠️ Ferramentas Necessárias

### Para Montagem
```
✅ Ferro de solda (25-40W)
✅ Solda 60/40 (0.6-0.8mm)
✅ Flux para solda
✅ Sugador de solda
✅ Morsa pequena ou fixador
✅ Óculos de aumento (opcional)
✅ Multímetro para testes
```

### Para Cabos/Conectores
```
✅ Cabo IPEX para antena
✅ Jumpers dupont (M-F, M-M)
✅ Protoboard para testes
✅ Wire wrap 30AWG para ligações
✅ Termo retrátil para isolação
```

## 📐 Layout de Montagem Sugerido

### Vista Frontal do TX DYU
```
                 ┌─ Antena 2.4GHz
                 │
┌───────────────┐│
│  ┌─────────┐  ││  ← Display OLED
│  │ DISPLAY │  ││     (GPIO 21/22)
│  │  OLED   │  ││
│  └─────────┘  ││
│               ││
│   ┌─────────┐ ││  ← ESP32 DevKit
│   │  ESP32  │ ││
│   │ DEVKIT  │ ││
│   └─────────┘ ││
│               ││  ← Módulo E28
│   ┌─────────┐ ├┘     (SPI connection)
│   │   E28   │─┘
│   │ SX1280  │
│   └─────────┘
│               
│     ┌─────┐     ← Joystick 5-way
│     │  ↑  │        (GPIO 35)
│     │ ←+→ │
│     │  ↓  │
│     └─────┘
└───────────────┘
```

### Vista Lateral (Empilhamento)
```
┌─────────────┐ ← Display OLED (topo)
├─────────────┤ 
├─────────────┤ ← ESP32 DevKit (meio)
├─────────────┤
├─────────────┤ ← Módulo E28 (base)
└─────────────┘
     ├─── Jumpers/fios de conexão
     ├─── Espaçadores 10-15mm
     └─── Suporte mecânico
```

## 🔍 Identificação de Problemas

### Sinais Visuais
```
✅ LED Power ON (ESP32): Azul aceso
✅ LED Status (GPIO15): Verde piscando = TX OK
✅ Display ligado: Logo ExpressLRS aparece
✅ Antena conectada: Conector IPEX fixo

❌ LED Power OFF: Problema alimentação
❌ LED Status vermelho: Erro comunicação
❌ Display apagado: Conexão I2C ou 5V
❌ Antena solta: Conectar bem o IPEX
```

### Pontos de Teste
```
TP1: 3.3V entre VCC e GND (ESP32)
TP2: 3.3V entre VCC e GND (E28)  
TP3: Clock SPI no GPIO18 (osciloscópio)
TP4: Sinal BUSY no GPIO21 (multímetro)
TP5: Continuidade antena (ohm meter)
```

## 📚 Recursos Adicionais

### Datasheets Oficiais
```
📄 ESP32-WROOM-32 Datasheet
📄 SX1280 Datasheet (Semtech)
📄 E28-2G4M27S Manual (EBYTE)
📄 SSD1306 OLED Controller
```

### Ferramentas Online
```
🌐 ExpressLRS Configurator
🌐 ESP32 Pinout Reference
🌐 SX1280 Calculator (RF settings)
🌐 PCB Layout Guidelines (IPC)
```

### Comunidades de Suporte
```
💬 ExpressLRS Discord
💬 ESP32 Forum (Espressif)
💬 RadioLabs Brasil
💬 FPV Brasil Telegram
```

---

**⚠️ Segurança:**
- Sempre use óculos de proteção ao soldar
- Trabalhe em área bem ventilada
- Verifique polaridade antes de conectar
- Teste continuidade antes de ligar
- Use antenas homologadas pela ANATEL

**📅 Criado:** 05 de Novembro de 2025  
**🎯 Para:** Projeto TX DYU ESP32 + E28 ExpressLRS  
**📖 Referência:** Configuração DIY completa