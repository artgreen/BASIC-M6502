We will be working to translate the 6502 assembly source code for MS Basic from the syntax for the ancient MACRO-10 assembler to the syntax for the ORCA-M assembler.  Please review the files in the docs/ directory for information on the format for both assemblers.  Then please review m6502.asm which is the original source code.  You and I will work together to update the m6502_orca.asm file.  I will ask you to transform the code in certain ways and you will do what I ask. Please review everything and let me know when you are ready to begin.

Here are some notes on translation to help you:
### 1. Assembly Directives and Pseudo-Ops
**MACRO-10 → ORCA-M**
- `TITLE` → Remove or convert to comment
- `SEARCH M6502` → Remove (library search directive)
- `SALL` → Remove (suppress listing directive)
- `SUBTTL` → Remove or convert to comment
- `PAGE` → Remove or convert to comment
- `RADIX 10` → Use decimal by default
- `ORG address` → `ORG address` (same)
- `BLOCK n` → `DS n` (define storage)
- `EXP value` → `DB value` (define byte)
- `ADR(label)` → `DW label` (define word address)
- `END label` → `END` (simplified)

### 2. Conditional Assembly
**MACRO-10 → ORCA-M**
- `IFE condition,<code>` → `IF condition = 0` / `ENDIF`
- `IFN condition,<code>` → `IF condition <> 0` / `ENDIF`
- `IF1,<code>` → `IF PASS = 1` / `ENDIF`
- `IF2,<code>` → `IF PASS = 2` / `ENDIF`

### 3. Symbol Definition
**MACRO-10 → ORCA-M**
- `SYMBOL==value` → `SYMBOL EQU value`
- `SYMBOL=value` → `SYMBOL EQU value`

### 4. Macro Definitions
**MACRO-10 → ORCA-M**
- `DEFINE name(params),<body>` → `name MAC` / `<<<` structure
- Parameter references need conversion
- Macro expansion syntax changes

### 5. Number Formats
**MACRO-10 → ORCA-M**
- `^Onnnnn` (octal) → `$nnnn` (hexadecimal equivalent)
- `^Dnnnn` (decimal) → `nnnn` (decimal, default)
- Need to convert all octal values to hex

### 6. String and Character Handling
**MACRO-10 → ORCA-M**
- `DT"string"` → Convert to proper string definition
- `ACRLF` macro → Convert to CR/LF bytes
- `PRINTX` → Remove (assembly-time print)

### 7. 6502 Instruction Mnemonics
**MACRO-10 → ORCA-M**
- `LDAI value` → `LDA #value` (immediate addressing)
- `LDYI value` → `LDY #value`
- `LDXI value` → `LDX #value`
- `CMPI value` → `CMP #value`
- `ADCI value` → `ADC #value`
- `SBCI value` → `SBC #value`
- `EORI value` → `EOR #value`
- All other 6502 instructions remain the same

### 8. Labels and Addressing
**MACRO-10 → ORCA-M**
- `LABEL:` → `LABEL` (colon optional in ORCA-M)
- `$Z::` → Convert to regular label
- Address expressions may need adjustment

### 9. Comments
**MACRO-10 → ORCA-M**
- `;comment` → `;comment` (same)
- `COMMENT *text*` → Convert to regular comments
