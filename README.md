<div align="center">

# 🐾 Dispenser Inteligente de Ração

**Alimentador automático para pets com MSP430F5529, sensor ultrassônico e LCD.**

*Projeto final de Sistemas Microprocessados, UnB.*

<br>

![MSP430](https://img.shields.io/badge/TI-MSP430F5529-E8A0BF?style=for-the-badge&logo=texasinstruments&logoColor=white)
![C](https://img.shields.io/badge/C-bare_metal-E8A0BF?style=for-the-badge&logo=c&logoColor=white)
![Code Composer Studio](https://img.shields.io/badge/IDE-Code_Composer_Studio-E8A0BF?style=for-the-badge)

[Como funciona](#-como-funciona) ·
[Hardware](#-hardware) ·
[Periféricos](#%EF%B8%8F-periféricos-do-msp430) ·
[Como compilar](#%EF%B8%8F-como-compilar)

</div>

<br>

## ✨ Como funciona

| | |
|---|---|
| 📏 **Detecção do pet** | Um HC-SR04 mede a distância continuamente. Se o pet chega a **10 cm ou menos**, a ração é liberada. |
| ⏱️ **Sem exagero** | Um relógio de 1 s no Timer B0 garante **pelo menos 1 minuto** entre duas liberações automáticas. |
| 🍽️ **Três porções** | Pelo LCD dá para escolher o modo **P**, **M** ou **G**, que define por quanto tempo o motor gira. |
| 🖐️ **Liberação manual** | O menu tem a opção "Liberar ração" para soltar uma porção na hora. |
| 📟 **Telemetria** | A distância medida sai pela UART, o que ajuda a calibrar e depurar. |

**Botões:** **S1** navega pelo menu (Modo P → Modo M → Modo G → Liberar ração) e **S2** confirma.

## 🔌 Hardware

| Componente | Ligação |
|---|---|
| MSP430F5529 LaunchPad | Microcontrolador |
| Sensor ultrassônico HC-SR04 | Trigger em **P2.0** (PWM do TA1), Echo em **P1.2** (captura do TA0) |
| Servo de rotação contínua | PWM de 20 ms em **P2.4** (TA2) |
| LCD 16×2 com módulo I2C PCF8574 | I2C (endereço 0x27 ou 0x3F) |
| Botões S1 / S2 da LaunchPad | **P2.1** / **P1.1** |

## ⚙️ Periféricos do MSP430

| Periférico | Uso no projeto |
|---|---|
| **Timer A0** (captura) | Mede a largura do pulso de Echo e converte em distância (`µs / 58 = cm`) |
| **Timer A1** (PWM) | Gera o pulso de Trigger de 10 µs a cada 60 ms |
| **Timer A2** (PWM) | Controla o servo |
| **Timer B0** (ACLK 32 kHz) | Relógio de 1 s que marca o intervalo de 1 minuto |
| **USCI I2C** | Comunicação com o LCD |
| **USCI UART** | Envio da distância para o computador |

```
msp430-pet-feeder/
├── main.c        # menu, botões e lógica de liberação
├── sensor.c / .h # HC-SR04 (TA0 + TA1)
├── motor.c / .h  # servo (TA2)
├── 3H.c / .h     # relógio de segundos/minutos (TB0)
├── lcd.c / .h    # LCD via PCF8574
├── i2c.c / .h    # driver I2C
├── UART.c / .h   # driver UART
├── gpio.c / .h   # botões e LEDs
└── utils.c / .h  # debounce e tempos de cada porção
```

## 🛠️ Como compilar

1. Instale o [Code Composer Studio](https://www.ti.com/tool/CCSTUDIO).
2. Clone o repositório:
   ```bash
   git clone https://github.com/anita211/msp430-pet-feeder.git
   ```
3. No CCS, use **File → Import → CCS Projects** e selecione a pasta.
4. Compile e grave na LaunchPad MSP430F5529 pelo debugger USB.

<div align="center">
<br>
<sub>Feito com 💗 por <a href="https://github.com/anita211">Anita Oliveira</a></sub>
</div>
