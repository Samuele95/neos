# Command Parser Specification

Defines how NFOS parses and validates shell-like commands.

---

## 1. Lexical Structure

### Token Types

| Token | Pattern | Example |
|-------|---------|---------|
| COMMAND_PREFIX | `nf` | `nf` |
| IDENTIFIER | `[a-zA-Z_][a-zA-Z0-9_]*` | `inject`, `cycle` |
| PATTERN_REF | `@[a-zA-Z_][a-zA-Z0-9_]*` | `@security` |
| FIELD_REF | `\$[a-zA-Z_][a-zA-Z0-9_]*` | `$reasoning` |
| COMMIT_REF | `#[a-f0-9]+` | `#abc123` |
| RELATIVE_REF | `~[0-9]+` | `~1`, `~3` |
| STRING | `"[^"]*"` or `'[^']*'` | `"sql injection"` |
| NUMBER | `-?[0-9]+(\.[0-9]+)?` | `0.85`, `-0.5` |
| FLAG | `--[a-zA-Z][a-zA-Z0-9-]*` | `--trace` |
| SHORT_FLAG | `-[a-zA-Z]` | `-v` |
| ASSIGNMENT | `[a-zA-Z_]+=[^ ]+` | `lambda=0.05` |
| OPERATOR | `[<>=!]+` | `>`, `>=`, `==` |

### Whitespace

- Spaces separate tokens
- Tabs equivalent to spaces
- Newlines terminate commands
- Multiple spaces collapse to one

### Comments

```
# This is a comment (not implemented in v1)
nf inject "pattern"  # inline comment (not implemented in v1)
```

---

## 2. Grammar

### Command Structure

```ebnf
command     ::= prefix verb [subverb] [arguments] [flags]
prefix      ::= "nf"
verb        ::= IDENTIFIER
subverb     ::= IDENTIFIER
arguments   ::= argument*
argument    ::= STRING | NUMBER | PATTERN_REF | FIELD_REF | COMMIT_REF | IDENTIFIER
flags       ::= flag*
flag        ::= FLAG [flag_value] | SHORT_FLAG [flag_value] | ASSIGNMENT
flag_value  ::= STRING | NUMBER | IDENTIFIER | PATTERN_REF | FIELD_REF
```

### Productions

```ebnf
(* Session commands *)
session_cmd ::= "nf" "session" ("new" STRING | "save" [STRING] | "load" STRING | "info")

(* Field operations *)
inject_cmd  ::= "nf" "inject" STRING [NUMBER] [inject_flags]
inject_flags ::= ("--into" FIELD_REF | "--tags" tag_list | "--position" coords)*
tag_list    ::= IDENTIFIER ("," IDENTIFIER)*
coords      ::= NUMBER "," NUMBER ["," NUMBER]

amplify_cmd ::= "nf" "amplify" PATTERN_REF [NUMBER] [amplify_flags]
amplify_flags ::= ("--max" NUMBER | "--resonance" | "--with" PATTERN_REF)*

attenuate_cmd ::= "nf" "attenuate" PATTERN_REF [NUMBER] [attenuate_flags]
attenuate_flags ::= ("--to" NUMBER | "--weak" | "--threshold" NUMBER)*

tune_cmd    ::= "nf" "tune" (ASSIGNMENT)+ ["--field" FIELD_REF | "--preset" IDENTIFIER]

collapse_cmd ::= "nf" "collapse" [collapse_flags]
collapse_flags ::= ("--strategy" IDENTIFIER | "--threshold" NUMBER | "--format" IDENTIFIER)*

resonate_cmd ::= "nf" "resonate" [PATTERN_REF PATTERN_REF] [resonate_flags]
resonate_flags ::= ("--all" | "--above" NUMBER | "--matrix" | "--explain")*

(* Dynamics *)
cycle_cmd   ::= "nf" "cycle" [NUMBER] [cycle_flags]
cycle_flags ::= ("--trace" | "--compact" | "--until" STRING)*

evolve_cmd  ::= "nf" "evolve" [evolve_flags]
evolve_flags ::= ("--target" NUMBER | "--max" NUMBER | "--trace" | "--until" STRING)*

step_cmd    ::= "nf" "step" [NUMBER] ["--phase" IDENTIFIER | "--verbose"]

reset_cmd   ::= "nf" "reset" [reset_flags]
reset_flags ::= ("--preserve" PATTERN_REF+ | "--preserve-attractors" | "--params" | "--confirm")*

(* Measurement *)
measure_cmd ::= "nf" "measure" ("coherence" | "energy" | "entropy" | "stability") [measure_flags]
measure_flags ::= ("--breakdown" | "--history" | "--clusters" | "--pattern" PATTERN_REF)*

attractor_cmd ::= "nf" "attractor" ("list" [attractor_list_flags] | "info" IDENTIFIER [attractor_info_flags] | "compare" IDENTIFIER IDENTIFIER)
attractor_list_flags ::= ("--all" | "--history")*
attractor_info_flags ::= ("--full" | "--history")*

basin_cmd   ::= "nf" "basin" PATTERN_REF [basin_flags]
basin_flags ::= ("--map" | "--boundary" | "--test" PATTERN_REF)*

state_cmd   ::= "nf" "state" [state_flags]
state_flags ::= ("--json" | "--algebraic" | "--compact" | "--field" FIELD_REF)*

(* Visualization *)
plot_cmd    ::= "nf" "plot" ("field" | "network" | "topology") [plot_flags]
plot_flags  ::= ("--style" IDENTIFIER | "--threshold" NUMBER | "--3d" | "--output" STRING)*

animate_cmd ::= "nf" "animate" "dynamics" [animate_flags]
animate_flags ::= ("--frames" NUMBER | "--output" STRING)*

export_cmd  ::= "nf" "export" IDENTIFIER [STRING]

(* Versioning *)
commit_cmd  ::= "nf" "commit" [STRING]
branch_cmd  ::= "nf" "branch" ("create" IDENTIFIER | "list" | "delete" IDENTIFIER)
checkout_cmd ::= "nf" "checkout" (IDENTIFIER | COMMIT_REF | RELATIVE_REF)
history_cmd ::= "nf" "history" [history_flags]
history_flags ::= ("--graph" | "--limit" NUMBER | "--all")*
diff_cmd    ::= "nf" "diff" [ref] [ref]
ref         ::= IDENTIFIER | COMMIT_REF | RELATIVE_REF
merge_cmd   ::= "nf" "merge" IDENTIFIER [merge_flags]
merge_flags ::= "--strategy" IDENTIFIER

(* Field management *)
field_cmd   ::= "nf" "field" ("create" IDENTIFIER [field_create_flags] | "list" | "activate" FIELD_REF | "delete" FIELD_REF)
field_create_flags ::= ("--params" STRING)*

route_cmd   ::= "nf" "route" FIELD_REF FIELD_REF ["--strength" NUMBER]
couple_cmd  ::= "nf" "couple" FIELD_REF FIELD_REF ["--gamma" NUMBER]

(* Autonomy *)
mode_cmd    ::= "nf" "mode" ("step" | "checkpoint" | "auto")
checkpoint_cmd ::= "nf" "checkpoint" (STRING | "list" | "clear")
proceed_cmd ::= "nf" "proceed" [NUMBER]
task_cmd    ::= "nf" "task" STRING

(* Interface *)
config_cmd  ::= "nf" "config" ("interface" IDENTIFIER | "set" IDENTIFIER value)
ask_cmd     ::= "nf" "ask" STRING
compute_cmd ::= "nf" "compute" expression
help_cmd    ::= "nf" "help" [IDENTIFIER]
```

---

## 3. Parsing Algorithm

### Phase 1: Tokenization

```
TOKENIZE(input):
  tokens ← []
  pos ← 0

  WHILE pos < length(input):
    SKIP_WHITESPACE()

    IF input[pos] == '"' OR input[pos] == "'":
      token ← READ_STRING()
    ELSE IF input[pos] == '@':
      token ← READ_PATTERN_REF()
    ELSE IF input[pos] == '$':
      token ← READ_FIELD_REF()
    ELSE IF input[pos] == '#':
      token ← READ_COMMIT_REF()
    ELSE IF input[pos] == '~':
      token ← READ_RELATIVE_REF()
    ELSE IF input[pos:pos+2] == '--':
      token ← READ_FLAG()
    ELSE IF input[pos] == '-' AND is_letter(input[pos+1]):
      token ← READ_SHORT_FLAG()
    ELSE IF is_digit(input[pos]) OR (input[pos] == '-' AND is_digit(input[pos+1])):
      token ← READ_NUMBER()
    ELSE IF is_letter(input[pos]) OR input[pos] == '_':
      token ← READ_IDENTIFIER_OR_ASSIGNMENT()
    ELSE:
      ERROR("Unexpected character: " + input[pos])

    tokens.append(token)

  RETURN tokens
```

### Phase 2: Parsing

```
PARSE(tokens):
  IF tokens[0].value != "nf":
    ERROR("Command must start with 'nf'")

  verb ← tokens[1]
  rest ← tokens[2:]

  MATCH verb:
    "session" → PARSE_SESSION(rest)
    "inject" → PARSE_INJECT(rest)
    "amplify" → PARSE_AMPLIFY(rest)
    "attenuate" → PARSE_ATTENUATE(rest)
    "tune" → PARSE_TUNE(rest)
    "collapse" → PARSE_COLLAPSE(rest)
    "resonate" → PARSE_RESONATE(rest)
    "cycle" → PARSE_CYCLE(rest)
    "evolve" → PARSE_EVOLVE(rest)
    "step" → PARSE_STEP(rest)
    "reset" → PARSE_RESET(rest)
    "measure" → PARSE_MEASURE(rest)
    "attractor" → PARSE_ATTRACTOR(rest)
    "basin" → PARSE_BASIN(rest)
    "state" → PARSE_STATE(rest)
    "plot" → PARSE_PLOT(rest)
    "animate" → PARSE_ANIMATE(rest)
    "export" → PARSE_EXPORT(rest)
    "commit" → PARSE_COMMIT(rest)
    "branch" → PARSE_BRANCH(rest)
    "checkout" → PARSE_CHECKOUT(rest)
    "history" → PARSE_HISTORY(rest)
    "diff" → PARSE_DIFF(rest)
    "merge" → PARSE_MERGE(rest)
    "field" → PARSE_FIELD(rest)
    "route" → PARSE_ROUTE(rest)
    "couple" → PARSE_COUPLE(rest)
    "mode" → PARSE_MODE(rest)
    "checkpoint" → PARSE_CHECKPOINT(rest)
    "proceed" → PARSE_PROCEED(rest)
    "task" → PARSE_TASK(rest)
    "config" → PARSE_CONFIG(rest)
    "ask" → PARSE_ASK(rest)
    "compute" → PARSE_COMPUTE(rest)
    "help" → PARSE_HELP(rest)
    ELSE → ERROR("Unknown command: " + verb)

  RETURN command_ast
```

### Phase 3: Validation

```
VALIDATE(ast):
  // Type checking
  FOR each argument IN ast.arguments:
    IF expected_type(argument) != actual_type(argument):
      ERROR("Type mismatch")

  // Range checking
  FOR each numeric IN ast.numeric_values:
    IF numeric.name == "strength" AND (numeric.value < 0 OR numeric.value > 1):
      ERROR("Strength must be in [0, 1]")
    // ... other ranges

  // Reference checking
  FOR each pattern_ref IN ast.pattern_refs:
    IF NOT EXISTS(pattern_ref) AND command.requires_existing:
      ERROR("Pattern not found: " + pattern_ref)

  FOR each field_ref IN ast.field_refs:
    IF NOT EXISTS(field_ref):
      ERROR("Field not found: " + field_ref)

  // Flag compatibility
  IF ast.has("--trace") AND ast.has("--compact"):
    WARNING("--trace and --compact are mutually exclusive, using --trace")

  RETURN validated_ast
```

---

## 4. Command AST Structure

```json
{
  "type": "command",
  "verb": "inject",
  "subverb": null,
  "arguments": [
    {"type": "string", "value": "security_concern"},
    {"type": "number", "value": 0.9}
  ],
  "flags": {
    "into": {"type": "field_ref", "value": "$reasoning"},
    "tags": {"type": "list", "value": ["vulnerability", "input"]}
  },
  "source": "nf inject \"security_concern\" 0.9 --into $reasoning --tags vulnerability,input"
}
```

---

## 5. Error Handling

### Error Types

| Error | Description | Example |
|-------|-------------|---------|
| `SyntaxError` | Malformed command | `nf inject "pattern` (unclosed quote) |
| `UnknownCommand` | Invalid verb | `nf injeect "pattern"` |
| `InvalidArgument` | Wrong type/format | `nf inject pattern abc` |
| `MissingArgument` | Required arg missing | `nf inject` (no pattern) |
| `InvalidFlag` | Unknown flag | `nf inject --unknown` |
| `ReferenceError` | Pattern/field not found | `nf amplify @nonexistent` |
| `RangeError` | Value out of range | `nf inject "p" 1.5` |
| `ConflictError` | Incompatible flags | `nf cycle --trace --quiet` |

### Error Messages

```
[ERROR] <ErrorType>: <message>
  Command: <original_command>
  Position: <caret_position>
  Suggestion: <how_to_fix>
```

### Example Errors

```
[ERROR] SyntaxError: Unterminated string
  Command: nf inject "sql injection
                     ^
  Suggestion: Add closing quote: nf inject "sql injection"

[ERROR] UnknownCommand: 'injec' is not a valid command
  Command: nf injec "pattern" 0.8
              ^~~~~
  Did you mean: inject?

[ERROR] InvalidArgument: Expected number, got string
  Command: nf inject "pattern" high
                               ^~~~
  Suggestion: Use numeric strength: nf inject "pattern" 0.8

[ERROR] MissingArgument: 'inject' requires a pattern name
  Command: nf inject
  Usage: nf inject "<pattern>" [strength]

[ERROR] ReferenceError: Pattern not found: @nonexistent
  Command: nf amplify @nonexistent
  Active patterns: @security, @injection, @input

[ERROR] RangeError: Strength must be between 0.0 and 1.0
  Command: nf inject "pattern" 1.5
                               ^~~
  Value 1.5 is out of range
```

---

## 6. Autocomplete Support

### Completion Triggers

| Context | Completions |
|---------|-------------|
| `nf ` | All verbs |
| `nf inject ` | Pattern name template |
| `nf amplify @` | Existing pattern names |
| `nf cycle --` | Valid flags for cycle |
| `nf field activate $` | Existing field names |
| `nf checkout ` | Branch names, commit hashes |

### Completion Algorithm

```
COMPLETE(partial_input):
  tokens ← TOKENIZE(partial_input)
  context ← DETERMINE_CONTEXT(tokens)

  MATCH context:
    "verb" → RETURN all_verbs.filter(startsWith(current))
    "pattern_ref" → RETURN active_patterns.map(p → "@" + p.name)
    "field_ref" → RETURN all_fields.map(f → "$" + f.name)
    "flag" → RETURN valid_flags_for_command(current_verb)
    "flag_value" → RETURN valid_values_for_flag(current_flag)
    "branch" → RETURN all_branches
    ELSE → RETURN []
```

---

## 7. Help Generation

### Dynamic Help

```
GENERATE_HELP(command):
  IF command == null:
    RETURN general_help()

  spec ← command_specs[command]

  output ← []
  output.append("nf " + command + " " + spec.usage)
  output.append("")
  output.append(spec.description)
  output.append("")

  IF spec.arguments:
    output.append("Arguments:")
    FOR arg IN spec.arguments:
      output.append("  " + arg.name + "  " + arg.description)

  IF spec.flags:
    output.append("")
    output.append("Flags:")
    FOR flag IN spec.flags:
      output.append("  --" + flag.name + "  " + flag.description)

  IF spec.examples:
    output.append("")
    output.append("Examples:")
    FOR example IN spec.examples:
      output.append("  " + example)

  RETURN output.join("\n")
```

---

## Related Documents

- `field-engine.md` - Command execution
- `../commands/index.md` - Command reference
- `state-manager.md` - State operations
