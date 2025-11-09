# Como Compilar com o Target Customizado DIY E28 I2C

## 🎯 Novo Target Disponível

Foi criado um novo target customizado para usar display OLED I2C:

- **Original:** `DIY2400 E28` → Display SPI (GPIO 2, 22, 32, 16, 33)
- **Novo:** `DIY2400 E28 I2C` → Display I2C (GPIO 21, 22)

## 📋 Arquivos Criados

### 1. Hardware JSON
```
src/hardware/TX/DIY 2400 E28 I2C.json
```
Configuração customizada com:
- `screen_type: 1` (I2C)
- `screen_sda: 21` (GPIO21)
- `screen_sck: 22` (GPIO22)

### 2. Entrada no Targets
```
src/hardware/targets.json
```
Nova entrada: `"e28-i2c"` na seção `"diy" → "tx_2400"`

## 🔧 Como Compilar

### Opção 1: Via ExpressLRS Configurator (Recomendado)

1. Abra o ExpressLRS Configurator
2. Em **Device target**, procure por:
   ```
   Manufacturer: DIY devices
   Device: DIY2400 E28 I2C (I2C OLED)
   ```
3. Configure suas opções (binding phrase, regulatory domain, etc.)
4. Clique em **Build** ou **Build & Flash**

### Opção 2: Via PlatformIO CLI

```bash
# Navegar para a pasta src
cd c:\dsn\ExpressLRS_Fork\src

# Compilar para UART (primeira instalação)
pio run -e Unified_ESP32_2400_TX_via_UART

# Upload via UART
pio run -t upload -e Unified_ESP32_2400_TX_via_UART --upload-port COM4

# Ou via WiFi (após primeira instalação)
pio run -t upload -e Unified_ESP32_2400_TX_via_WIFI --upload-address <IP_DO_TX>
```

### Opção 3: Especificar Manualmente o Target

Se precisar compilar o target específico:

```bash
# Definir o target antes de compilar
export EXPRESSLRS_TARGET="DIY_2400_TX_ESP32_SX1280_E28"

# Para o target I2C customizado, o sistema usará automaticamente 
# o arquivo "DIY 2400 E28 I2C.json" quando você selecionar
# "DIY2400 E28 I2C" no Configurator
```

## 📊 Diferenças entre os Targets

| Característica | DIY2400 E28 (Original) | DIY2400 E28 I2C (Novo) |
|----------------|------------------------|------------------------|
| **Display** | SPI OLED | I2C OLED |
| **Pinos** | CS=2, DC=22, MOSI=32, RST=16, SCK=33 | SDA=21, SCL=22 |
| **Vantagens** | Mais rápido | Menos fios, mais simples |
| **GPIOs livres** | Menos | GPIO 2, 16, 32, 33 disponíveis |
| **Lua Name** | `DIY2400 E28` | `DIY2400 E28 I2C` |

## 🔍 Verificar Target Compilado

Após compilar, você pode verificar qual configuração foi usada:

```bash
# Ver o JSON que foi embutido no firmware
pio run -e Unified_ESP32_2400_TX_via_UART -t size

# Verificar logs de compilação
# Procure por: "Using layout file: DIY 2400 E28 I2C.json"
```

## 🛠️ Troubleshooting

### Display não funciona após compilar

1. **Verificar se compilou o target correto:**
   - No Configurator, confirme que selecionou **"DIY2400 E28 I2C"**
   - Não confunda com "DIY2400 E28" (sem I2C)

2. **Verificar conexões físicas:**
   ```
   Display    →    ESP32
   VCC        →    3.3V
   GND        →    GND
   SDA        →    GPIO 21
   SCL        →    GPIO 22
   ```

3. **Verificar tipo de display:**
   - Suportado: SSD1306 128x64 (I2C address 0x3C ou 0x3D)
   - Não usar display SPI neste target!

### Como voltar para o target original

Se quiser usar o target SPI original:

1. No Configurator, selecione **"DIY2400 E28"** (sem I2C)
2. Recompile e faça upload
3. Reconecte o display nas configurações SPI

## 📚 Arquivos de Documentação

- `ESP32_Driver_Fix_Guide.md` - Problemas com driver CP2102
- `TX_DYU_ESP32_E28_Config.md` - Configuração resumida
- `src/TX_DYU_ESP32_E28_Configuracao_Completa.md` - Documentação completa
- `src/ESP32_E28_SX1280_Circuit_Diagram.md` - Diagramas de circuito
- `src/Joystick_5Way_ADC_Explanation.md` - Explicação do joystick
- `src/Visual_Guide_ESP32_E28_Components.md` - Guia visual

## ✅ Checklist de Compilação

- [ ] Clonar/atualizar o repositório
- [ ] Garantir que `src/hardware/` contém os arquivos targets
- [ ] Selecionar target correto no Configurator: **"DIY2400 E28 I2C"**
- [ ] Configurar binding phrase
- [ ] Configurar regulatory domain
- [ ] Compilar firmware
- [ ] Fazer upload via UART ou WiFi
- [ ] Testar display OLED I2C

## 🔗 Links Úteis

- Repositório ExpressLRS: https://github.com/ExpressLRS/ExpressLRS
- Repositório Targets: https://github.com/ExpressLRS/targets
- Seu Fork: https://github.com/edilsoncorrea/ExpressLRS/tree/diy_esp32_devkit_E28

---

**Criado em:** 09 de Novembro de 2025  
**Branch:** diy_esp32_devkit_E28  
**Target:** Unified_ESP32_2400_TX_via_UART
