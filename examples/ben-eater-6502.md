# Case Study: Ben Eater — Building a 6502 Computer

Detailed verification of Script Snap processing on low-level assembly and hardware compilation toolchains.

---

## 1. Video Profile

* **Context**: Low-level 6502 assembly programming, EEPROM address boundaries, custom hardware peripheral access, and transitioning from hardcoded memory locations to a modern compiler suite (cc65: ca65 assembler and ld65 linker) to build a BIOS.
* **Length**: ~2 hours.

---

## 2. Generic Transcript Limitations

A generic automatic speech recognition (ASR) engine processing this audio produces:
- Flat blocks of text lacking paragraph breaks.
- Raw assembly instructions and linker settings inline without syntax highlighting.
- Severe phonetic mishearings of historical retro-computing systems and assembly directives.
- Zero structured references to register operations or physical pin configurations.

---

## 3. Terminology Correction Log

Script Snap identified and resolved the following phonetic anomalies:

| Raw ASR Text | Corrected Entity | Context / Explanation |
|---|---|---|
| "wasmon" | `WozMon` | Steve Wozniak's Apple I system monitor program |
| "Brentwood computer" | `breadboard computer` | Custom hardware computer built on an electronic breadboard |
| "dot org" | `.org` | Assembler directive for hardcoding absolute memory origins |
| "c c sixty five" | `cc65` | Compiler and toolchain suite for 6502-based processors |
| "l d sixty five" | `ld65` | Linker tool within the cc65 compiler suite |

---

## 4. Engineering Impact: Why It Matters

When programming close to the metal, documentation accuracy is critical:
- **Obscured References**: Referencing `wasmon` prevents developers from discovering WozMon resources, breaking historical and technical context.
- **Undefined Directives**: Attempting to compile `dot org` halts the assembler. Modern linkers require the correct `.org` syntax or segment configs.
- **Brittle Architecture**: In linker configs, layout segments must align with physical address lines. Transcription errors in address values will crash the system immediately upon booting.

---

## 5. Extracted Structured Outputs

### A. Modularity Paradigm Shift Analysis
Script Snap generated a clear comparison mapping the modernization of low-level software design:

| Feature | Manual Assembly (.org) | Linker-Based (ld65) |
|---|---|---|
| **Memory Layout** | Bound to source code lines | Decoupled into external configuration file |
| **Modularity** | Complex, requires manual address adjustments | Seamless linking of independent objects |
| **Reset Vectors** | Manually padded to end of ROM | Automated layout via segments |

### B. Linker Configuration Blueprint (`bios.config`)
Decoupled memory management configuration extracted from the tutorial:

```ld65
MEMORY {
    RAM: start = $0000, size = $4000, type = rw;
    ROM: start = $8000, size = $8000, type = ro, fill = yes;
    RESETVEC: start = $FFFA, size = 6, type = ro, fill = yes;
}

SEGMENTS {
    BIOS: load = ROM, type = ro;
    WASMON: load = ROM, type = ro, start = $FF00;
    RESETVEC: load = RESETVEC, type = ro;
}
```

### C. Abracted BIOS Segment Interface
Extracted ACIA serial port communication subroutines:

```asm
.segment "BIOS"

character_in:
    lda ACIA_STATUS
    and #$08
    beq character_in
    lda ACIA_DATA
    rts

character_out:
    pha
wait_out:
    lda ACIA_STATUS
    and #$02
    beq wait_out
    pla
    sta ACIA_DATA
    rts
```
