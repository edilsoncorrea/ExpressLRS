# TX DYU ESP32 DevKit com E28 LoRa SX1280 - Configurações ExpressLRS

## Resumo da Configuração

Este documento contém todas as informações relevantes para compilar o firmware ExpressLRS para o TX DYU com ESP32 DevKit e módulo E28 LoRa SX1280.

## Target de Compilação

- **Nome do Target:** `Unified_ESP32_2400_TX_via_UART`
- **Nome no Lua:** `DIY2400 E28`
- **Produto:** DIY ESP32 E28 2.4GHz TX
- **Arquivo de Layout:** `DIY 2400 E28.json`
- **Plataforma:** ESP32
- **Chip de Rádio:** SX1280 (2.4GHz)
- **Firmware Base:** `Unified_ESP32_2400_TX`

## Comandos de Compilação

### Via UART (Recomendado)
```bash
pio run -e Unified_ESP32_2400_TX_via_UART
```

### Via WiFi
```bash
pio run -e Unified_ESP32_2400_TX_via_WIFI
```

### Via ETX (EdgeTX)
```bash
pio run -e Unified_ESP32_2400_TX_via_ETX
```

## Displays Compatíveis

O TX DYU ESP32 E28 suporta os seguintes tipos de display:

### 1. I2C OLED (SSD1306 128x64)
- **Tipo:** `screen_type = 1`
- **Interface:** I2C
- **Resolução:** 128x64 pixels
- **Pinout:**
  - **SDA:** GPIO 32
  - **SCK (SCL):** GPIO 33
  - **RST:** GPIO 16 (opcional)
- **Biblioteca:** U8G2_SSD1306_128X64_NONAME_F_HW_I2C

### 2. SPI OLED (SSD1306 128x64)
- **Tipo:** `screen_type = 2`
- **Interface:** SPI
- **Resolução:** 128x64 pixels
- **Pinout:**
  - **MOSI:** GPIO 32
  - **SCK:** GPIO 33
  - **CS:** GPIO 2
  - **DC:** GPIO 22
  - **RST:** GPIO 16
- **Biblioteca:** U8G2_SSD1306_128X64_NONAME_F_4W_SW_SPI

### 3. SPI OLED Small (SSD1306 128x32)
- **Tipo:** `screen_type = 3`
- **Interface:** SPI
- **Resolução:** 128x32 pixels
- **Pinout:**
  - **MOSI:** GPIO 32
  - **SCK:** GPIO 33
  - **CS:** GPIO 2
  - **DC:** GPIO 22
  - **RST:** GPIO 16
- **Biblioteca:** U8G2_SSD1306_128X32_UNIVISION_F_4W_SW_SPI

### 4. SPI TFT (ST7735 160x80)
- **Tipo:** `screen_type = 4`
- **Interface:** SPI
- **Resolução:** 160x80 pixels
- **Pinout:**
  - **MOSI:** GPIO 32
  - **SCK:** GPIO 33
  - **CS:** GPIO 2
  - **DC:** GPIO 22
  - **RST:** GPIO 16
  - **BL (Backlight):** Opcional
- **Biblioteca:** Arduino_ST7735

### Configuração no Hardware JSON

Para habilitar um display, configure o parâmetro `screen_type` no arquivo `DIY 2400 E28.json`:

```json
{
    "screen_type": 2,  // 0=None, 1=I2C OLED, 2=SPI OLED, 3=SPI OLED Small, 4=SPI TFT
    "screen_cs": 2,
    "screen_dc": 22,
    "screen_mosi": 32,
    "screen_rst": 16,
    "screen_sck": 33,
    "screen_sda": 32
}
```

### Recursos dos Displays

**OLED Features:**
- ✅ Tela inicial com logo ExpressLRS
- ✅ Status de conexão RF
- ✅ Taxa de pacotes (packet rate)
- ✅ Potência de transmissão
- ✅ Status de telemetria
- ✅ Modo WiFi/configuração
- ✅ Status de binding
- ✅ Menu de configuração
- ✅ Link statistics

**TFT Features (Adicional):**
- ✅ Interface colorida
- ✅ Ícones gráficos
- ✅ Interface mais rica
- ✅ Suporte a backlight
- ✅ Todos os recursos do OLED

### Recomendações de Display

**Para uso básico:**
- **SSD1306 128x64 OLED (I2C)** - Mais simples, menos fios

**Para interface completa:**
- **SSD1306 128x64 OLED (SPI)** - Melhor performance que I2C

**Para interface colorida:**
- **ST7735 TFT 160x80** - Interface mais moderna e colorida

**Para projetos compactos:**
- **SSD1306 128x32 OLED (SPI)** - Versão menor, ideal para espaços reduzidos

### Módulos Recomendados

#### OLED I2C (mais fácil de conectar)
- **0.96" OLED 128x64 I2C SSD1306**
- **Vantagens:** Apenas 4 fios (VCC, GND, SDA, SCL)
- **Desvantagens:** Velocidade menor que SPI

#### OLED SPI (melhor performance)
- **0.96" OLED 128x64 SPI SSD1306**
- **1.3" OLED 128x64 SPI SSH1106** (compatível)
- **Vantagens:** Velocidade maior, mais confiável
- **Desvantagens:** Mais fios para conectar

#### TFT Colorido (interface premium)
- **0.96" TFT 160x80 ST7735**
- **1.14" TFT 240x135 ST7789** (pode necessitar adaptação)
- **Vantagens:** Interface colorida, backlight
- **Desvantagens:** Maior consumo de energia

#### OLED Pequeno (projetos compactos)
- **0.91" OLED 128x32 SPI SSD1306**
- **Vantagens:** Tamanho reduzido
- **Desvantagens:** Menos espaço para informações

## Configuração de Hardware (DIY 2400 E28.json)

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
    "power_lna_gain": 12,
    "power_min": 0,
    "power_high": 4,
    "power_max": 4,
    "power_default": 2,
    "power_control": 0,
    "power_values": [-15,-11,-8,-5,-1],
    "led_rgb": 15,
    "led_rgb_isgrb": true,
    "screen_cs": 2,
    "screen_dc": 22,
    "screen_mosi": 32,
    "screen_rst": 16,
    "screen_sck": 33,
    "screen_sda": 32,
    "screen_type": 0,
    "use_backpack": false,
    "debug_backpack_baud": 460800,
    "debug_backpack_rx": 3,
    "debug_backpack_tx": 1,
    "misc_fan_en": 17
}
```

## Pinout ESP32 para E28 LoRa SX1280

### Comunicação SPI com E28
- **MISO:** GPIO 19
- **MOSI:** GPIO 23
- **SCK:** GPIO 18
- **NSS (CS):** GPIO 5
- **RST:** GPIO 14
- **BUSY:** GPIO 21
- **DIO1:** GPIO 4

### Controle de Potência
- **RX Enable:** GPIO 27
- **TX Enable:** GPIO 26
- **LNA Gain:** GPIO 12

### Display (Opcional)
- **CS:** GPIO 2
- **DC:** GPIO 22
- **MOSI:** GPIO 32
- **RST:** GPIO 16
- **SCK:** GPIO 33
- **SDA:** GPIO 32

### Outros
- **LED RGB:** GPIO 15 (GRB format)
- **Ventilador:** GPIO 17
- **Serial RX/TX:** GPIO 13

## Configurações de Potência

- **Potência Mínima:** 0 (-15 dBm)
- **Potência Máxima:** 4 (-1 dBm)
- **Potência Padrão:** 2 (-8 dBm)
- **Valores Disponíveis:** [-15, -11, -8, -5, -1] dBm

## Características do Target

- ✅ **Plataforma:** ESP32
- ✅ **Chip de Rádio:** SX1280 (2.4GHz)
- ✅ **Features:** Suporte a ventilador (fan)
- ✅ **Métodos de Upload:** UART, WiFi
- ✅ **Versão Mínima:** 3.0.0
- ✅ **LED RGB:** Suportado (formato GRB)
- ✅ **Display:** Suportado (OLED/TFT)
- ✅ **Backpack:** Não utilizado

## Informações de Compilação (Última Build)

### Status da Compilação
- ✅ **Status:** Sucesso
- ⏱️ **Tempo de Compilação:** 7 minutos e 11 segundos
- 📅 **Data:** 4 de Novembro de 2025

### Uso de Memória
- **RAM:** 21.7% utilizada (71,016 bytes de 327,680 bytes)
- **Flash:** 82.1% utilizada (1,613,521 bytes de 1,966,080 bytes)

### Arquivos Gerados
- **Firmware Binary:** `.pio\build\Unified_ESP32_2400_TX_via_UART\firmware.bin`
- **ELF File:** `.pio\build\Unified_ESP32_2400_TX_via_UART\firmware.elf`

## Configuração no ExpressLRS Configurator

Quando usar o ExpressLRS Configurator, procure por:
- **Fabricante:** DIY
- **Modelo:** DIY ESP32 E28 2.4GHz TX
- **Nome Lua:** DIY2400 E28

## Configuração no targets.json

```json
"e28": {
    "product_name": "DIY ESP32 E28 2.4GHz TX",
    "lua_name": "DIY2400 E28",
    "layout_file": "DIY 2400 E28.json",
    "features": ["fan"],
    "upload_methods": ["uart", "wifi"],
    "min_version": "3.0.0",
    "platform": "esp32",
    "firmware": "Unified_ESP32_2400_TX",
    "prior_target_name": "DIY_2400_TX_ESP32_SX1280_E28"
}
```

## Notas Importantes

1. **Módulo E28:** Este target é específico para módulos E28 com chip SX1280
2. **Frequência:** Funciona em 2.4GHz (não compatível com 900MHz)
3. **ESP32 DevKit:** Compatível com placas ESP32 padrão
4. **Ventilador:** Suporte nativo para controle de ventilador no GPIO 17
5. **Upload:** Recomendado via UART para primeira instalação

## Troubleshooting

### Problemas Comuns
- Verificar conexões SPI entre ESP32 e E28
- Confirmar alimentação adequada (3.3V para E28)
- Verificar se o GPIO do ventilador não está em conflito
- Para upload via WiFi, ensure that the device is in WiFi mode

### Verificação de Hardware
```
ESP32 -> E28 
GPIO 19 -> MISO
GPIO 23 -> MOSI  
GPIO 18 -> SCK
GPIO 5  -> NSS
GPIO 14 -> RST
GPIO 21 -> BUSY
GPIO 4  -> DIO1
3.3V    -> VCC
GND     -> GND
```

### Fiação dos Displays

#### OLED I2C (SSD1306 128x64)
```
ESP32    -> Display
GPIO 32  -> SDA
GPIO 33  -> SCL
GPIO 16  -> RST (opcional)
3.3V     -> VCC
GND      -> GND
```

#### OLED SPI (SSD1306 128x64/128x32)
```
ESP32    -> Display
GPIO 32  -> MOSI (SDA)
GPIO 33  -> SCK (SCL)
GPIO 2   -> CS
GPIO 22  -> DC
GPIO 16  -> RST
3.3V     -> VCC
GND      -> GND
```

#### TFT SPI (ST7735 160x80)
```
ESP32    -> Display
GPIO 32  -> MOSI
GPIO 33  -> SCK
GPIO 2   -> CS
GPIO 22  -> DC
GPIO 16  -> RST
3.3V     -> VCC
GND      -> GND
GPIO XX  -> BL (Backlight) - Opcional
```

---

**Documento gerado em:** 4 de Novembro de 2025  
**Versão ExpressLRS:** Master branch  
**Target configurado:** DIY ESP32 E28 2.4GHz TX

## Navegação nos Menus

### Controles de Navegação

O ExpressLRS TX suporta dois tipos de controle para navegação nos menus:

#### 1. Joystick Analógico (Five-Way)
- **GPIO:** Definido em `HARDWARE_joystick`
- **Tipo:** ADC (Analógico)
- **Valores:** 6 posições (UP, DOWN, LEFT, RIGHT, CENTER, IDLE)

#### 2. Botões Digitais (Five-Way)
- **GPIO 1:** `HARDWARE_five_way1`
- **GPIO 2:** `HARDWARE_five_way2` 
- **GPIO 3:** `HARDWARE_five_way3`
- **Tipo:** Digital com pull-up

### Comandos de Navegação

| Ação | Função | Descrição |
|------|--------|-----------|
| **↑ UP** | Anterior | Move para o item anterior do menu |
| **↓ DOWN** | Próximo | Move para o próximo item do menu |
| **→ RIGHT** | Entrar | Entra no submenu ou confirma seleção |
| **← LEFT** | Voltar | Volta para o menu anterior |
| **CENTER** | Entrar | Entra no submenu ou confirma seleção |
| **LONG PRESS CENTER** | Ação especial | Função especial (ex: salvar e enviar) |

### Estrutura de Menus

#### Menu Principal
1. **Packet Rate** - Taxa de pacotes
2. **Switch Mode** - Modo de chaveamento
3. **Antenna** - Configuração de antena
4. **Power** - Potência de transmissão
5. **Telemetry** - Configuração de telemetria
6. **Power Save** - Economia de energia
7. **Smart Fan** - Controle de ventilador
8. **BLE Joystick** - Joystick Bluetooth
9. **Bind** - Vinculação com RX
10. **WiFi Update** - Atualização via WiFi
11. **VTX Admin** - Configuração de VTX

#### Submenus de Potência
- **Power Max** - Potência máxima
- **Power Dynamic** - Potência dinâmica

#### Submenus VTX
- **VTX Band** - Banda do VTX
- **VTX Channel** - Canal do VTX
- **VTX Power** - Potência do VTX
- **VTX Pitmode** - Modo pit do VTX

### Eventos de Navegação

#### Eventos Básicos
- `EVENT_ENTER` - Pressão curta do botão central
- `EVENT_UP` - Botão para cima
- `EVENT_DOWN` - Botão para baixo
- `EVENT_LEFT` - Botão esquerda
- `EVENT_RIGHT` - Botão direita

#### Eventos de Pressão Longa
- `EVENT_LONG_ENTER` - Pressão longa do centro (>1000ms)
- `EVENT_LONG_UP` - Pressão longa para cima
- `EVENT_LONG_DOWN` - Pressão longa para baixo
- `EVENT_LONG_LEFT` - Pressão longa esquerda
- `EVENT_LONG_RIGHT` - Pressão longa direita

### Estados do Sistema

#### Estados Principais
- `STATE_SPLASH` - Tela inicial
- `STATE_IDLE` - Tela principal (status)
- `STATE_LINKSTATS` - Estatísticas de link

#### Estados de Menu
- `STATE_PACKET` - Menu de taxa de pacotes
- `STATE_POWER` - Menu de potência
- `STATE_BIND` - Menu de binding
- `STATE_WIFI` - Menu WiFi
- `STATE_VTX` - Menu VTX

### Timeouts

- **Menu Principal:** 20 segundos de inatividade retorna ao idle
- **Submenus:** 20 segundos de inatividade volta ao menu principal
- **Tela Idle:** Atualiza a cada 100ms
- **Pressão Longa:** 1000ms para ativar

### Configuração no Hardware

Para habilitar controles de menu, configure no `DIY 2400 E28.json`:

```json
{
    // Joystick analógico (recomendado)
    "joystick": [GPIO_PIN],
    "joystick_values": [valor_up, valor_down, valor_left, valor_right, valor_center, valor_idle],
    
    // OU botões digitais
    "five_way1": [GPIO_PIN],
    "five_way2": [GPIO_PIN], 
    "five_way3": [GPIO_PIN]
}
```

### Dicas de Uso

1. **Navegação Rápida:** Use as setas para navegar rapidamente
2. **Voltar:** LEFT sempre volta ao menu anterior
3. **Confirmação:** CENTER ou RIGHT confirma ações
4. **Pressão Longa:** Use para ações especiais (ex: salvar e enviar VTX)
5. **Timeout:** Menus retornam automaticamente ao idle após 20s
6. **Status:** A tela idle mostra status em tempo real

### Troubleshooting Navegação

1. **Botões não respondem:**
   - Verificar GPIOs configurados corretamente
   - Confirmar pull-ups habilitados
   - Testar com multímetro

2. **Joystick não funciona:**
   - Verificar valores ADC no array `joystick_values`
   - Calibrar valores para sua configuração específica
   - Verificar conexão ADC

3. **Menu trava:**
   - Timeout automático após 20s
   - Reiniciar o TX se necessário

---