# TX DYU ESP32 DevKit com E28 LoRa SX1280 - Configuração Completa

## 📋 Informações do Hardware

- **Placa:** ESP32 DevKit (ESP32-D0WD-V3)
- **Módulo LoRa:** E28-2G4M27S (SX1280, 2.4GHz)
- **USB-UART:** CP2102 Silicon Labs
- **Target ExpressLRS:** DIY ESP32 E28 2.4GHz TX
- **Firmware:** Unified_ESP32_2400_TX_via_UART

**📸 Guia visual dos componentes:** Ver arquivo `Visual_Guide_ESP32_E28_Components.md`

## 🔌 Pinagem ESP32 → E28 LoRa Module

**📐 Diagrama completo:** Ver arquivo `ESP32_E28_SX1280_Circuit_Diagram.md`

### Conexões SPI do Rádio
```
ESP32 GPIO    →    E28 Pin    →    Função
----------------------------------------
GPIO 23       →    MOSI       →    SPI Data Out
GPIO 19       →    MISO       →    SPI Data In  
GPIO 18       →    SCK        →    SPI Clock
GPIO 5        →    NSS        →    SPI Chip Select
GPIO 14       →    RST        →    Reset
GPIO 4        →    DIO1       →    Interrupt
GPIO 21       →    BUSY       →    Status
```

### Alimentação e Controle
```
ESP32         →    E28        →    Função
----------------------------------------
3.3V          →    VCC        →    Alimentação
GND           →    GND        →    Terra
GPIO 27       →    RXEN       →    RX Enable
GPIO 26       →    TXEN       →    TX Enable
```

**⚠️ Importante:** 
- Usar apenas **3.3V** (nunca 5V!)
- Conectar antena 2.4GHz ao conector IPEX
- Manter trilhas SPI curtas (<5cm)
- Adicionar capacitores de bypass (100nF + 10µF)

## 🕹️ Conexão de Joystick 5-Way para Navegação

**🧮 Como funciona:** Ver explicação técnica em `Joystick_5Way_ADC_Explanation.md`

### Pino Recomendado
```
GPIO 35 (ADC1_CH7) → Joystick 5-way
```

### Esquema de Conexão
```
Joystick 5-way → Rede de Resistores → ESP32 GPIO 35

UP     ──┤ R1 ├──┐
DOWN   ──┤ R2 ├──┤
LEFT   ──┤ R3 ├──┼── GPIO 35 (ADC)
RIGHT  ──┤ R4 ├──┤ 
CENTER ──┤ R5 ├──┘
                 │
                 ├── 10kΩ ── 3.3V (pull-up)
                 │
               GND (comum)
```

### 🔬 Princípio de Funcionamento

**Um único ADC detecta 5 direções** através de **divisor de tensão**:

1. **Cada botão** tem um **resistor diferente** (470Ω, 820Ω, 1.2kΩ, etc.)
2. **Quando pressionado**, cria uma **tensão única** no ADC
3. **Software compara** a tensão lida com valores calibrados
4. **Identifica qual direção** foi pressionada

**Exemplo:**
- UP (470Ω) → ADC lê ~1905 (1.53V)
- DOWN (820Ω) → ADC lê ~1160 (0.95V)  
- IDLE (10kΩ) → ADC lê ~4095 (3.3V)

### Configuração no JSON
Adicionar no arquivo `DIY 2400 E28.json`:
```json
{
    "joystick": 35,
    "joystick_values": [1905, 1160, 580, 2580, 0, 4095]
}
```

**Array significa:** `[UP, DOWN, LEFT, RIGHT, CENTER, IDLE]`

### Exemplos de Valores de Outros TXs
- **EMAX:** `[1905, 1160, 580, 2580, 0, 4095]`
- **HGLRC:** `[3276, 2072, 1377, 2730, 0, 4095]`
- **Namimno:** `[1850, 900, 490, 1427, 0, 2978]`
- **Radiomaster:** `[3227, 0, 1961, 2668, 1290, 4095]`

## 📺 Opções de Display

### Display OLED I2C (Configurado)
```
ESP32 GPIO    →    Display    →    Função
----------------------------------------
GPIO 22       →    SCL        →    I2C Clock
GPIO 21       →    SDA        →    I2C Data
3.3V          →    VCC        →    Alimentação
GND           →    GND        →    Terra
```

**Status:** ✅ Configuração atualizada no firmware para I2C nos pinos 21/22

**Configuração JSON atual:**
```json
{
    "screen_sck": 22,
    "screen_sda": 21,
    "screen_type": 1
}
```

### Troubleshooting do Display OLED

**Se o display não funcionar após o upload:**

1. **Verificar alimentação:** Display deve receber 3.3V (não 5V!)
2. **Verificar conexões:** 
   - SCL → GPIO 22
   - SDA → GPIO 21
   - VCC → 3.3V
   - GND → GND
3. **Tipos de display suportados:**
   - SSD1306 (128x64) - Mais comum
   - SH1106 (128x64) - Alternativa
4. **Endereços I2C:** 0x3C ou 0x3D (detectado automaticamente)

### Display OLED SPI (Configuração Original)
```
ESP32 GPIO    →    Display    →    Função
----------------------------------------
GPIO 18       →    SCK        →    SPI Clock
GPIO 23       →    MOSI       →    SPI Data
GPIO 5        →    CS         →    Chip Select
GPIO 16       →    DC         →    Data/Command
GPIO 17       →    RST        →    Reset
```

### Display TFT SPI
```
ESP32 GPIO    →    Display    →    Função
----------------------------------------
GPIO 18       →    SCK        →    SPI Clock
GPIO 23       →    MOSI       →    SPI Data
GPIO 5        →    CS         →    Chip Select
GPIO 2        →    DC         →    Data/Command
GPIO 4        →    RST        →    Reset
GPIO 15       →    BL         →    Backlight
```

## 🎛️ Sticks RC Adicionais (Futuro)

### Pinos ADC Disponíveis
```
ESP32 GPIO    →    Uso Sugerido
--------------------------------
GPIO 34       →    Stick Direito X (Aileron)
GPIO 36       →    Stick Direito Y (Elevator)
GPIO 37       →    Stick Esquerdo X (Rudder)  
GPIO 38       →    Stick Esquerdo Y (Throttle)
GPIO 32       →    Potenciômetro 1
GPIO 33       →    Potenciômetro 2
```

**Nota:** Requer modificação no código para implementar.

## 💡 LEDs e Indicadores

### LED RGB (WS2812)
```
GPIO 12 → LED RGB (Opcional)
```

### LEDs Simples
```
GPIO 25 → LED Status TX
GPIO 26 → LED Status Bind
```

## 🔘 Botões Adicionais

### Botão de Bind
```
GPIO 0 → Botão Bind (Boot button do ESP32)
```

### Botões Auxiliares
```
GPIO 13 → Botão 1
GPIO 15 → Botão 2
GPIO 2  → Botão 3
```

## 🔊 Buzzer (Opcional)

```
GPIO 25 → Buzzer PWM
```

## ⚡ Especificações de Potência

### Níveis de Potência do SX1280
```
Nível    dBm    mW
-----------------
0        -18    ~0.016
1        -15    ~0.032  
2        -12    ~0.063
3        -9     ~0.126
4        -4     ~0.398
5        -1     ~0.794
6        3      ~2.0
```

### Configuração no JSON
```json
{
    "power_min": 0,
    "power_high": 6,
    "power_max": 6, 
    "power_default": 2,
    "power_values": [-18,-15,-12,-9,-4,-1,3]
}
```

## 🛠️ Compilação

### Comando PlatformIO
```bash
cd c:\dsn\ExpressLRS\src
pio run -e Unified_ESP32_2400_TX_via_UART
```

### Upload via UART
```bash
pio run -t upload -e Unified_ESP32_2400_TX_via_UART --upload-port COM4
```

### Configuração no ExpressLRS Configurator
1. Selecionar **Device target:** DIY ESP32 E28 2.4GHz TX
2. Configurar **Regulatory Domain:** conforme sua região
3. Definir **Binding Phrase** customizada
4. Ajustar **Performance Options** conforme necessário

## 🎯 Navegação nos Menus

### Controles do Joystick
- **UP/DOWN:** Navegar entre itens do menu
- **LEFT/RIGHT:** Alterar valores/opções  
- **CENTER:** Selecionar/confirmar
- **LONG PRESS CENTER:** Voltar ao menu anterior

### Estrutura dos Menus
```
Main Menu
├── WiFi Update
├── Bind
├── Band
├── Packet Rate  
├── Telemetry Ratio
├── Switch Mode
├── Model Match
├── TX Power
└── Options
    ├── VTX Administrator
    ├── Fan Threshold
    ├── Motion Detection
    ├── Diversity
    └── BLE Joystick
```

## 🔧 Solução de Problemas

### Driver CP2102
Se o ESP32 não for detectado:
1. Baixar driver oficial Silicon Labs CP210x
2. Desinstalar driver antigo no Device Manager
3. Instalar novo driver
4. Reconectar ESP32

### Verificação de Conexões
```bash
# Testar comunicação
esptool.py --port COM4 chip_id

# Verificar flash
esptool.py --port COM4 flash_id
```

### LED Status
- **Verde piscando:** TX funcionando normalmente
- **Azul piscando:** Modo bind ativo
- **Vermelho:** Erro de comunicação

## 📊 Informações do Chip

### ESP32 Detectado
```
Chip: ESP32-D0WD-V3 (revision 3)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal: 40MHz
MAC: c0:49:ef:65:49:8c
```

### Partições Flash
```
app0:    0x10000   (1MB)   - Firmware principal
app1:    0x110000  (1MB)   - Firmware backup (OTA)
spiffs:  0x210000  (1.5MB) - Sistema de arquivos
```

## 🎯 Próximos Passos

1. **✅ Compilar firmware** - Concluído
2. **✅ Upload firmware** - Concluído  
3. **🔄 Conectar módulo E28** - Seguir pinagem acima
4. **🔄 Adicionar joystick** - GPIO 35 recomendado
5. **🔄 Instalar display** - OLED I2C nos pinos 21/22
6. **🔄 Calibrar joystick** - Ajustar valores ADC
7. **🔄 Testar bind** - Com receptor ExpressLRS
8. **🔄 Configurar bind phrase** - Via interface web
9. **🔄 Ajustar potência** - Conforme necessário
10. **🔄 Adicionar sticks RC** - Desenvolvimento futuro

## 📝 Arquivos de Configuração

### Localização
```
Arquivo JSON: c:\dsn\ExpressLRS\src\hardware\TX\DIY 2400 E28.json
Target: c:\dsn\ExpressLRS\src\hardware\targets.json
```

### Backup da Configuração
```json
{
    "serial_rx": 13,
    "serial_tx": 13,
    "radio_busy": 21,
    "radio_dio1": 4,
    "radio_miso": 19,
    "radio_mosi": 23,
    "radio_nss": 5,
    "radio_rst": 14,
    "radio_sck": 18,
    "power_rxen": 27,
    "power_txen": 26,
    "power_min": 0,
    "power_high": 6,
    "power_max": 6,
    "power_default": 2,
    "power_values": [-18,-15,-12,-9,-4,-1,3],
    "use_backpack": false
}
```

---

**Data de criação:** 05 de Novembro de 2025  
**ExpressLRS Version:** Master Branch  
**Status:** Firmware compilado e enviado com sucesso ✅