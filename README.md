# Huffman Coding DSD Project

## Project Description

This project implements a frequency-based fixed Huffman encoder in Verilog for the Digilent Basys3 FPGA board. The design was developed as a Digital System Design Laboratory project and is structured as a complete hardware datapath with control logic, memory, sorting, code assignment, and seven-segment display output.

Unlike a software-only Huffman encoder, this version is built to run directly on FPGA hardware. It accepts 8-bit ASCII symbols through the Basys3 switches, registers input with push-button control, computes symbol frequencies, assigns Huffman codes based on frequency rank, and displays the current symbol and encoded output on the onboard 7-segment display.

## Key Features

- Basys3 FPGA implementation targeting the Artix-7 XC7A35T-1CPG236 device
- 8-bit ASCII input through switches
- Button-based input registration and encode control
- Frequency counting for four symbols
- Hardware sorting and rank-based Huffman code assignment
- Shift-register and bit-counter based serial code generation
- 4-digit seven-segment display output
- Synchronous memory and fully modular Verilog design

## Design Overview

The top-level design is organized under `top_module` and connects the following Verilog blocks:

- `control_unit.v` - Moore FSM that orchestrates the full encode flow
- `memory_unit.v` - synchronous RAM used for symbol storage and working memory
- `frequency_counter.v` - counts symbol occurrences
- `alu.v` - comparator logic for sorting and control decisions
- `code_assigner.v` - maps symbol rank to Huffman code and code length
- `bit_counter.v` - tracks remaining bits in the current codeword
- `shift_register.v` - shifts out Huffman code bits serially
- `seg7_display.v` - drives the 7-segment display
- `top_module.v` - integrates the complete system and button synchronizers

## How It Works

1. Reset the board with `BTNC`.
2. Set a symbol on `SW7:SW0`.
3. Press `BTNL` to register that symbol.
4. Repeat the process for the remaining symbols in your input sequence.
5. Press `BTNR` to start Huffman encoding.
6. Use `BTNU` to advance through the encoded symbols when needed.
7. The 7-segment display shows the selected symbol and its Huffman code.
8. `LED15` turns on when encoding is complete.

## Pin Mapping

The design is constrained for the Basys3 board using `basys3.xdc`.

- `clk`  -> `W5`
- `reset` -> `U18` (`BTNC`)
- `valid` -> `W19` (`BTNL`)
- `encode` -> `T17` (`BTNR`)
- `next_sym_btn` -> `T18` (`BTNU`)
- `symbol[7:0]` -> `SW7:SW0`
- `seg[6:0]` -> `W7, W6, U8, V8, U5, V5, U7`
- `an[3:0]` -> `U2, U4, V4, W4`
- `done` -> `L1` (`LED15`)

## Project Structure

- `create_project.tcl` - Vivado project rebuild script
- `Huffman_coding_dsd.xpr` - Vivado project file
- `Huffman_coding_dsd.srcs/` - synthesizable source and constraint files
- `docs/` - project notes, report source, and supporting documents

## Getting Started

### In Vivado

1. Open Vivado.
2. Run the Tcl script:

```tcl
source create_project.tcl
```

3. Run synthesis and implementation.
4. Generate the bitstream.
5. Program the Basys3 board.

### Command-Line Batch Mode

```bash
vivado -mode batch -source create_project.tcl
```

## Notes

- The source design is written in Verilog.
- The design is intended for the Basys3 FPGA board.
- The `docs/` folder is ignored in version control so local reports and generated documents stay out of future commits.

## Authors

- Md. Tanjimul Hasan
- Md. Abu Loman Hossain Shuvo

## License

No explicit license has been added yet.