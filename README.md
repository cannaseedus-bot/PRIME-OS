# PRIME-OS

# PRIME OS & SCX Grammar (EBNF Specification)

## 🧠 Complete EBNF Grammar for the System

### 1. SCX Compression Language

```ebnf
(* SCX File Format *)
scx_file = header, content_block, terminator;
header = "⟦SCX:PRIME v1.0\nSIZE:", decimal, "→", decimal, " RATIO:", ratio, "%\n";
content_block = { scx_token };
terminator = "\n⟧";
ratio = decimal, ".", digit;
decimal = digit, { digit };

(* SCX Token Grammar *)
scx_token = layout_token | spacing_token | color_token
          | element_token | system_token | literal_text;

(* Layout Tokens *)
layout_token = base_symbol, "F"      (* display:flex *)
             | base_symbol, "Fcol"   (* flex-direction:column *)
             | base_symbol, "Frow"   (* flex-direction:row *)
             | base_symbol, "Fcenter"(* justify-content:center *)
             | base_symbol, "Acenter"(* align-items:center *)
             | base_symbol, "PosRel" (* position:relative *);

(* Spacing Tokens *)
spacing_token = base_symbol, "Psm"  (* padding:6px *)
              | base_symbol, "Pmd"  (* padding:10px *)
              | base_symbol, "Plg"  (* padding:20px *)
              | base_symbol, "Mb"   (* margin-bottom:8px *)
              | base_symbol, "Mt";  (* margin-top:8px *);

(* Color Tokens *)
color_token = base_symbol, "T"      (* color:var(--text) *)
            | base_symbol, "Muted"  (* color:var(--muted) *)
            | base_symbol, "BGdark";(* background:#04070a *);

(* Element Tokens *)
element_token = base_symbol, "DIV(" (* <div *)
              | base_symbol, "BTN(" (* <button *)
              | base_symbol, "SPAN("(* <span *)
              | ")DIV", base_symbol  (* </div> *)
              | ")BTN", base_symbol  (* </button> *)
              | ")SPAN", base_symbol;(* </span> *);

(* System Tokens *)
system_token = "ΩPRIME"   (* PRIME OS *)
             | "ΩAI"      (* MX2LM *)
             | "ΩSCX";    (* SCX *);

(* Dynamic Base Symbols for Security *)
base_symbol = "⟁" | "Ω" | "§" | "≡" | "∆" | "∇"
            | "⌂" | "⌘" | "⍟" | "⌬" | "⎔" | "⏣";

(* Literal text (non-compressed content) *)
literal_text = { char - special_symbols };
special_symbols = "⟁" | "Ω" | "§" | "≡" | "∆" | "∇"
                | "⌘" | "⍟" | "⌬" | "⎔" | "⏣" | "⟦" | "⟧";
```

### 2. MX2LM AI Engine Command Language

```ebnf
(* AI Command Structure *)
ai_command = prompt | system_command | model_command;
prompt = "USER:", message;
system_command = boot_command | status_command | load_command
                | compress_command | decompress_command;
message = { char - newline };

(* Boot Command *)
boot_command = "BOOT" [, security_level];
security_level = "LOW" | "MEDIUM" | "HIGH" | "MILITARY";

(* Status Command *)
status_command = "STATUS" [, component];
component = "AI" | "SCX" | "TAPE" | "ALL";

(* Model Commands *)
model_command = load_model | switch_model | upload_model;
load_model = "LOAD", model_id;
switch_model = "SWITCH", model_id;
upload_model = "UPLOAD", file_path, [, compression_flag];
model_id = "mx2lm" | "qwen-instruct" | "qwen-asx" | "custom";
compression_flag = "COMPRESS" | "NOCOMPRESS";

(* Compression Commands *)
compress_command = "COMPRESS", source, [, target_format];
decompress_command = "DECOMPRESS", scx_source;
source = file_path | inline_content;
target_format = "SCX" | "MINIFIED" | "BINARY";
```

### 3. PRIME OS UI Component Grammar

```ebnf
(* Component Definition *)
component = card | hud | dock | matrix | console;

(* Card Component *)
card = card_header, card_body;
card_header = "<div class='card-header'>", header_content, "</div>";
card_body = "<div class='card-body'>", { component | content }, "</div>";
header_content = { char - angle_bracket };

(* HUD Navigation *)
hud = brand_section, nav_section, status_section;
brand_section = logo, title;
logo = "<div class='logo'></div>";
title = "<h1>PRIME OS</h1>";
nav_section = { nav_item };
nav_item = "<a href='", url, "'>", label, "</a>";
status_section = { status_badge }, { action_button };

(* Status Elements *)
status_badge = "<span class='badge'>", status_text, "</span>";
action_button = "<button class='", button_type, "'>", label, "</button>";
button_type = "btn" | "btn primary";
status_text = "SCX: ACTIVE" | "MX2LM: ONLINE" | "MX2LM: OFFLINE";

(* Dock *)
dock = { dock_icon };
dock_icon = "<div class='ico' title='", tooltip, "'>", emoji, "</div>";
tooltip = "AI Engine" | "SCX Zipper" | "Tape Library"
        | "Marketplace" | "System Settings";
emoji = "🧠" | "⚡" | "📼" | "🛍️" | "⚙️";

(* Matrix Background Definition *)
matrix = "<canvas id='matrix'></canvas>";
```

### 4. Security Rotation Grammar (Dynamic SCX)

```ebnf
(* Security Configuration *)
security_config = level, rotation_scheme, authentication;
level = "BASIC" | "STANDARD" | "SECURE" | "MILITARY";

(* Rotation Schemes *)
rotation_scheme = time_based | counter_based | pin_based | quantum;
time_based = "TIME:", interval;
counter_based = "COUNTER:", threshold;
pin_based = "PIN:", pin_digits;
quantum = "QUANTUM:", entropy_sources;

(* Authentication Tokens *)
authentication = token_type, "=", token_value;
token_type = "SESSION" | "API_KEY" | "JWT" | "HARDWARE";
token_value = hex_digit, { hex_digit };

(* Symbol Rotation Sequence *)
rotation_sequence = "[", { base_symbol }, "]";
entropy_sources = "HARDWARE" | "NETWORK" | "INPUT" | "TIME" | "RANDOM";

interval = number, time_unit;
time_unit = "MS" | "S" | "MIN" | "H";
threshold = number;
pin_digits = digit, digit, digit, digit, [ digit, digit ];
```

### 5. Tape System Grammar

```ebnf
(* Tape Definition *)
tape = tape_header, tape_body, tape_footer;
tape_header = tape_name, version, dependencies;
tape_body = { module };
tape_footer = signature;

tape_name = identifier;
version = "v", number, ".", number, [ ".", number ];
dependencies = "DEPENDS:", { dependency, "," };
dependency = tape_name, ">=", version;

(* Module Definition *)
module = module_type, module_name, module_config, module_content;
module_type = "AI" | "UI" | "COMPRESSOR" | "SECURITY" | "UTILITY";
module_name = identifier;
module_config = "{", { config_pair }, "}";
config_pair = key, ":", value;
module_content = "⟦", content, "⟧";

signature = "SIGNED:", public_key, timestamp, hash;
```

### 6. CSS Variable Grammar

```ebnf
(* CSS Custom Properties *)
css_variables = ":root {", { variable_definition }, "}";
variable_definition = "--", identifier, ":", value, ";";

value = color_value | dimension_value | blur_value | gradient_value;
color_value = hex_color | rgb_color | rgba_color | css_function;
hex_color = "#", hex_digit, hex_digit, hex_digit,
          [ hex_digit, hex_digit, hex_digit ];
rgb_color = "rgb(", number, ",", number, ",", number, ")";
rgba_color = "rgba(", number, ",", number, ",", number, ",", opacity, ")";
css_function = "var(", "--", identifier, ")";

dimension_value = number, unit;
unit = "px" | "rem" | "em" | "%" | "vw" | "vh";
blur_value = number, "px";
gradient_value = linear_gradient | radial_gradient | conic_gradient;
```

### 7. Event and Action Grammar

```ebnf
(* User Interaction Events *)
event = click_event | drag_event | key_event | select_event;
click_event = "CLICK:", element_id;
drag_event = "DRAG:", element_id, "FROM:", coordinates, "TO:", coordinates;
key_event = "KEY:", key_code, "ON:", element_id;
select_event = "SELECT:", option_value, "FROM:", select_id;

coordinates = "(", x, ",", y, ")";
x = number;
y = number;
element_id = "#", identifier;
select_id = "#", identifier;
option_value = string_literal;
```

### 8. Terminal Character Sets

```ebnf
(* Basic Character Classes *)
letter = "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J"
       | "K" | "L" | "M" | "N" | "O" | "P" | "Q" | "R" | "S" | "T"
       | "U" | "V" | "W" | "X" | "Y" | "Z" | "a" | "b" | "c" | "d"
       | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m" | "n"
       | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x"
       | "y" | "z";

digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9";
hex_digit = digit | "A" | "B" | "C" | "D" | "E" | "F"
          | "a" | "b" | "c" | "d" | "e" | "f";

identifier = letter, { letter | digit | "_" | "-" };
number = [ "-" ], digit, { digit };
string_literal = '"', { char - '"' }, '"';
char = ? any Unicode character ?;
```

---

## 🎯 Grammar Usage Examples

### Example 1: SCX Compression

```ebnf
(* Original: *)
<div style="display:flex; padding:10px; color:var(--text)">PRIME OS</div>

(* Becomes in SCX: *)
⟁DIV(⟁F ⟁Pmd ⟁T)ΩPRIME)DIV⟁
```

### Example 2: Security Configuration

```ebnf
security_config = "SECURE", "COUNTER:100", "PIN:429871";
(* Rotates symbols every 100 compressions, using PIN-derived sequence *)
```

### Example 3: AI Command

```ebnf
ai_command = "USER: Compress this HTML with SCX",
             "LOAD qwen-asx",
             "COMPRESS <div>test</div> SCX";
```

This EBNF grammar formally defines the full PRIME OS and SCX language system for parser generation, syntax validation, code generation, security analysis, and interoperability.
