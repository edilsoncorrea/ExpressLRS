# Como GPIO 35 (ADC) Controla Joystick 5-Way - Explicação Técnica

## 🧮 Princípio: Divisor de Tensão com Resistores

### 🔌 Conceito Fundamental

Um joystick 5-way **NÃO É** um joystick analógico tradicional. É na verdade **5 botões independentes** conectados através de uma **rede de resistores** que cria **tensões únicas** para cada direção.

### 📊 Como Funciona

```
                    3.3V
                     │
                   10kΩ (Pull-up)
                     │
    UP────┤ 470Ω ├───┤
                     │
  DOWN────┤ 820Ω ├───┤
                     │── GPIO 35 (ADC)
  LEFT────┤ 1.2kΩ├───┤
                     │
 RIGHT────┤ 390Ω ├───┤
                     │
CENTER────┤ 10kΩ├────┤
                     │
                    GND
```

### ⚡ Cálculo das Tensões

Quando **nenhum botão** é pressionado:
- **Resistência total:** 10kΩ (apenas pull-up)
- **Tensão:** 3.3V
- **ADC:** 4095 (máximo)

Quando **UP** é pressionado:
- **Resistência:** 470Ω || 10kΩ = ~450Ω
- **Divisor:** 450Ω / (450Ω + 10kΩ) ≈ 0.465
- **Tensão:** 3.3V × 0.465 ≈ 1.53V
- **ADC:** ~1905

Quando **DOWN** é pressionado:
- **Resistência:** 820Ω || 10kΩ = ~760Ω  
- **Tensão:** 3.3V × 0.288 ≈ 0.95V
- **ADC:** ~1160

E assim por diante...

## 📈 Tabela Completa de Valores

```
┌─────────┬─────────┬─────────┬─────────┬────────────┐
│ Botão   │ R (Ω)   │ Tensão  │ ADC     │ Tolerância │
├─────────┼─────────┼─────────┼─────────┼────────────┤
│ IDLE    │ 10000   │ 3.30V   │ 4095    │ ±50        │
│ UP      │ 470     │ 1.53V   │ 1905    │ ±100       │
│ DOWN    │ 820     │ 0.95V   │ 1160    │ ±80        │
│ LEFT    │ 1200    │ 0.47V   │ 580     │ ±60        │
│ RIGHT   │ 390     │ 2.07V   │ 2580    │ ±120       │
│ CENTER  │ 10000   │ 0.00V   │ 0       │ ±30        │
└─────────┴─────────┴─────────┴─────────┴────────────┘
```

## 🔬 Detecção no Software

### Algoritmo de Reconhecimento

```cpp
int readJoystick() {
    uint16_t adcValue = analogRead(GPIO_PIN_JOYSTICK);
    
    // Array dos valores esperados: [UP, DOWN, LEFT, RIGHT, CENTER, IDLE]
    uint16_t joyValues[] = {1905, 1160, 580, 2580, 0, 4095};
    uint16_t fuzzValues[] = {100, 80, 60, 120, 30, 50}; // Tolerâncias
    
    for (int i = 0; i < 6; i++) {
        if (adcValue >= (joyValues[i] - fuzzValues[i]) && 
            adcValue <= (joyValues[i] + fuzzValues[i])) {
            return i; // 0=UP, 1=DOWN, 2=LEFT, 3=RIGHT, 4=CENTER, 5=IDLE
        }
    }
    
    return 5; // Default: IDLE
}
```

### Mapeamento para Ações

```cpp
enum JoystickAction {
    INPUT_KEY_UP_PRESS = 0,
    INPUT_KEY_DOWN_PRESS = 1,
    INPUT_KEY_LEFT_PRESS = 2,
    INPUT_KEY_RIGHT_PRESS = 3,
    INPUT_KEY_OK_PRESS = 4,
    INPUT_KEY_NO_PRESS = 5
};

const uint8_t IDX_TO_INPUT[5] = {
    INPUT_KEY_UP_PRESS,
    INPUT_KEY_DOWN_PRESS, 
    INPUT_KEY_LEFT_PRESS,
    INPUT_KEY_RIGHT_PRESS,
    INPUT_KEY_OK_PRESS
};
```

## 🛠️ Implementação Física

### Joystick 5-Way Real

```
Vista Superior:
     ┌─────────┐
     │    ↑    │ ← UP (470Ω)
     │  ← ● → │ ← LEFT (1.2kΩ) / RIGHT (390Ω)
     │    ↓    │ ← DOWN (820Ω)
     └─────────┘
         ↑
      CENTER (10kΩ) - Push central

Vista dos Pinos:
┌─────────────────┐
│ U  L  C  R  D   │ ← 5 terminais soldados
│ ○  ○  ○  ○  ○   │
└─────────────────┘

U = UP, L = LEFT, C = CENTER, R = RIGHT, D = DOWN
```

### Circuito Interno do Joystick

```
Cada direção é um botão SPST (momentâneo):

UP ────○── (quando pressionado, conecta ao comum)
LEFT ──○── (quando pressionado, conecta ao comum)
CENTER─○── (quando pressionado, conecta ao comum)
RIGHT──○── (quando pressionado, conecta ao comum)  
DOWN───○── (quando pressionado, conecta ao comum)
        │
     COMUM ── Vai para GPIO 35
```

## 🎯 Vantagens do Sistema

### ✅ Benefícios

1. **Economia de pinos:** 5 funções em 1 GPIO
2. **Confiabilidade:** Valores únicos para cada posição
3. **Simplicidade:** Não precisa de multiplexador
4. **Custo baixo:** Apenas resistores comuns
5. **Debounce natural:** Filtro analógico suaviza transições

### ⚠️ Limitações

1. **Pressões simultâneas:** Não detecta múltiplas direções
2. **Tolerância de componentes:** Resistores ±5% podem alterar valores
3. **Ruído ADC:** Precisa de filtro em software
4. **Calibração:** Valores podem variar entre unidades

## 🔧 Calibração Prática

### Como Encontrar os Valores

```cpp
// Código para calibração (usar no setup())
void calibrateJoystick() {
    Serial.println("Calibração do Joystick:");
    Serial.println("Pressione cada direção e anote os valores:");
    
    while (true) {
        int adcValue = analogRead(35);
        Serial.print("ADC: ");
        Serial.println(adcValue);
        delay(100);
    }
}

Procedimento:
1. Upload código de calibração
2. Abrir Serial Monitor
3. Pressionar UP → anotar valor
4. Pressionar DOWN → anotar valor  
5. Pressionar LEFT → anotar valor
6. Pressionar RIGHT → anotar valor
7. Pressionar CENTER → anotar valor
8. Não pressionar nada → anotar valor (IDLE)
9. Atualizar array no JSON
```

### Exemplo de Valores Reais

Diferentes fabricantes usam resistores diferentes:

```cpp
// EMAX TX
uint16_t joystick_values[] = {1905, 1160, 580, 2580, 0, 4095};

// HGLRC TX  
uint16_t joystick_values[] = {3276, 2072, 1377, 2730, 0, 4095};

// Namimno TX
uint16_t joystick_values[] = {1850, 900, 490, 1427, 0, 2978};

// Radiomaster TX
uint16_t joystick_values[] = {3227, 0, 1961, 2668, 1290, 4095};
```

## 📊 Comparação com Outras Abordagens

### Matriz de Botões (Alternativa)

```
Usando 3 GPIOs para 5 botões:

GPIO1 ──┤    ├── UP
        │    ├── DOWN
GPIO2 ──┤    ├── LEFT  
        │    ├── RIGHT
GPIO3 ──┤    ├── CENTER

Vantagem: Múltiplas pressões simultâneas
Desvantagem: Usa mais pinos
```

### Encoder Rotativo (Alternativa)

```
Usando 3 GPIOs:

GPIO_A ── Encoder A
GPIO_B ── Encoder B  
GPIO_C ── Push Button

Vantagem: Navegação infinita
Desvantagem: Menos intuitivo
```

## 🧪 Teste e Debug

### Verificação de Funcionamento

```cpp
// Teste simples no loop()
void loop() {
    int joy = readJoystick();
    
    switch (joy) {
        case 0: Serial.println("UP"); break;
        case 1: Serial.println("DOWN"); break;
        case 2: Serial.println("LEFT"); break;
        case 3: Serial.println("RIGHT"); break;
        case 4: Serial.println("CENTER"); break;
        case 5: Serial.println("IDLE"); break;
    }
    
    delay(200);
}
```

### Problemas Comuns

```
❌ Valores erráticos → Verificar soldas dos resistores
❌ Não detecta algumas direções → Resistor incorreto  
❌ Múltiplas leituras → Debounce insuficiente
❌ Deriva nos valores → Interferência ou ruído
```

---

**Resumo:** O GPIO 35 consegue controlar um joystick 5-way porque **cada direção tem um resistor diferente** que cria uma **tensão única** quando pressionada. O ADC lê essa tensão e o software identifica qual botão foi pressionado comparando com valores pré-calibrados. É um sistema elegante que economiza pinos e funciona muito bem na prática! 

**📅 Atualizado:** 06 de Novembro de 2025