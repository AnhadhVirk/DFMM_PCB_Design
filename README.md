# UQ Racing | Driverless Fault Management Module

The Driverless Fault Management Module (DFMM) is a four-layer PCB for the University of Queensland’s Formula SAE autonomous race car. It brings vehicle fault signals, two CAN buses and the autonomous-system shutdown circuit (SDC) onto one board. An STM32 handles the programmable functions. A separate set of logic gates, a flip-flop and a relay implement the hardwired portion of the shutdown circuit.

**My contribution.** I led development of the module for UQ Racing, redesigned the four-layer PCB to improve signal isolation, and wrote bare-metal firmware for the safety system. This repository contains the Altium hardware design. Firmware source is outside this hardware archive.

## Start here

Open [2025 DFMM V1.PrjPcb](2025%20DFMM%20V1.PrjPcb) in Altium Designer with all the files in the same directory. Start with the [top-level schematic](DFMM_2024.SchDoc) for the vehicle connections and power supply, then open the [2025 V2.4 PCB layout](DFMM_2025_V2.4.PcbDoc) to see how the circuits are placed and routed.

The filenames span two design years. The 2025 project still uses `DFMM_2024.SchDoc` as its top sheet and points to `DFMM_2025_V2.4.PcbDoc` as its board. The separate [2024 PCB layout](DFMM_2024.PcbDoc) is included for comparison.

## Hardware walkthrough

| Schematic | What to look for |
| --- | --- |
| [Vehicle interface and power](DFMM_2024.SchDoc) | The main harness connector (`J1`), protected 12 V input, 5 V and 3.3 V rails, and input conditioning. The sheet labels fault inputs from the battery management system (BMS), brake system plausibility device (BSPD) and insulation monitoring device (IMD). |
| [Microcontroller and communications](Microcontroller.SchDoc) | The STM32F415RGT6 (`U13`), UCC2946 supervisor (`U14`), programming headers and rotary-switch inputs. Two SN65HVD233QDRQ1 transceivers (`U16`, `U17`) connect the MCU’s two CAN interfaces to the vehicle buses. |
| [Hardwired shutdown logic](Non_Prog_Logic.SchDoc) | The logic gates, flip-flop and MOSFET relay driver around the 12 V shutdown relay (`K1`). Follow the named signals between this sheet and the top sheet to trace the relationship between the shutdown command, fault indication and relay contacts. |

### Two signal paths to trace

- **Fault input.** Find `BSPD_FAULT` at the vehicle interface, then follow `12V_BSPD_FAULT` and `3V_BSPD_FLT` across the sheets to see how a vehicle-level signal reaches the MCU.
- **CAN interface.** Find `CAN1_TX` and `CAN1_RX` on the MCU sheet and follow them through the transceiver to `CAN1_P` and `CAN1_N` at the harness. CAN2 has its own transceiver and connector nets.

## Supporting files

- [DFMM.PcbLib](DFMM.PcbLib) contains project PCB footprints.
- [2025 DFMM V1.OutJob](2025%20DFMM%20V1.OutJob) contains Altium output-job settings. Generated Gerbers, drill files and test reports are outside this source archive.
