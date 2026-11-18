# SimpleBlinky - STM32 Nucleo LED Blink (QA documented)

Minimal STM32 HAL firmware that blinks the green user LED (LD1, PB0) of a Nucleo-144 board.
This repository is used for the Project Management activity
"GitHub-Based QA Documentation and Problem Solving" (course 2307476T, CO2).

Base project: public SimpleBlinky repository (STM32CubeMX and ST HAL, Copyright STMicroelectronics 2023).
[Add the original author or repository link here if you know it.]

## Supported boards
| Board | MCU | Config file | Linker script | Notes |
|-------|-----|-------------|---------------|-------|
| NUCLEO-F207ZG | STM32F207ZGT6 | `SimpleBlinky.ioc` | `STM32F207ZGTX_FLASH.ld` | `Core/Src/main.c` is generated for this board (HSI 16 MHz, PLL, 120 MHz) |
| NUCLEO-F446ZE | STM32F446ZET6 | `BlinkyF4.ioc` | `STM32F446ZETX_FLASH.ld` | 168 MHz in the .ioc; regenerate the code in CubeMX |
| NUCLEO-F756ZG | STM32F756ZGT6 | `BlinkyF7.ioc` | `STM32F756ZGTX_FLASH.ld` | regenerate the code in CubeMX |

## Pin map (Nucleo-144)
| Signal | Pin | Use |
|--------|-----|-----|
| LD1 (green) | PB0 | blink output |
| LD2 (blue) | PB7 | unused |
| LD3 (red) | PB14 | fault indicator (`Error_Handler`) |
| USER_Btn | PC13 | reserved (future feature) |
| USART3 TX/RX | PD8 / PD9 | ST-Link virtual COM port (not used yet) |

## Behaviour (requirement R1)
LD1 blinks with a period of **1000 ms**, ON for **125 ms** (12.5 % duty).
Timing comes from the SysTick millisecond counter (`HAL_GetTick()`), so it does not
depend on the CPU clock or the compiler optimisation level (see issue #1).
If a fatal error happens, LD3 (red) blinks fast and the program stops (see issue #2).

## Build and run
1. Open STM32CubeIDE and import the folder (File > Import > Existing Projects into Workspace).
2. Select the build configuration (Debug or Release) and the board.
3. Build, then flash with the ST-Link (Run > Debug).
4. Check LD1 with a stopwatch: 10 blinks in about 10 seconds.

## QA process
- Defects are logged in **Issues** using the bug report template, with labels
  `bug`, `robustness`, `documentation`, `refactor`, `enhancement` and a `severity:` label.
- One branch per fix (`fix/issue-N-...`); the commit message ends with `Fixes #N`.
- Pull requests use the PR template (checklist, test evidence, self-review).
- The QA checklist and the test log are in `docs/`.

## Repository layout
```
Core/Inc, Core/Src   application and HAL glue code
Drivers/             CMSIS and STM32 HAL drivers
docs/                QA_checklist.md, test_log.md
.github/             issue and pull request templates
*.ioc                CubeMX configurations (one per board)
README.md
```
