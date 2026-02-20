# Interface Translation

Specification for translating between algebraic, semantic, and shell command perspectives.

---

## 1. Overview

Translation enables seamless switching between interfaces, allowing users to:
- Express ideas in their preferred notation
- View results in any perspective
- Learn mappings between representations
- Verify understanding across notations

---

## 2. Translation Layers

### 2.1 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    User Interface                            │
├──────────────┬──────────────────┬──────────────────────────┤
│   Semantic   │    Algebraic     │         Shell            │
│  Interface   │    Interface     │       Interface          │
├──────────────┴──────────────────┴──────────────────────────┤
│                  Translation Layer                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                 │
│  │ Semantic │  │ Algebraic│  │  Shell   │                 │
│  │ Parser   │  │ Parser   │  │ Parser   │                 │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘                 │
│       │             │             │                        │
│       └─────────────┼─────────────┘                        │
│                     ▼                                       │
│            ┌────────────────┐                              │
│            │ Canonical AST  │                              │
│            └────────┬───────┘                              │
│                     │                                       │
│       ┌─────────────┼─────────────┐                        │
│       ▼             ▼             ▼                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                 │
│  │ Semantic │  │ Algebraic│  │  Shell   │                 │
│  │Generator │  │Generator │  │Generator │                 │
│  └──────────┘  └──────────┘  └──────────┘                 │
├─────────────────────────────────────────────────────────────┤
│                    Field Engine                             │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 Canonical AST

All interfaces translate to a common abstract syntax tree:

```json
{
  "type": "operation",
  "operation": "inject",
  "arguments": {
    "pattern": {
      "type": "pattern_ref",
      "name": "security"
    },
    "activation": {
      "type": "number",
      "value": 0.85
    },
    "tags": {
      "type": "list",
      "values": ["critical"]
    }
  },
  "modifiers": {},
  "source": {
    "interface": "semantic",
    "original": "Focus strongly on security"
  }
}
```

---

## 3. Translation Mappings

### 3.1 Core Operations

| Semantic | Algebraic | Shell | Canonical |
|----------|-----------|-------|-----------|
| "Focus on X" | ι(X, 0.8) | `nf inject X 0.8` | `{op: inject, pattern: X, activation: 0.8}` |
| "Emphasize X" | α(X, 1.5) | `nf amplify X` | `{op: amplify, pattern: X, factor: 1.5}` |
| "De-emphasize X" | ν(X, 0.5) | `nf attenuate X` | `{op: attenuate, pattern: X, factor: 0.5}` |
| "Think about this" | Φⁿ(F) | `nf cycle n` | `{op: cycle, count: n}` |
| "What emerges?" | κ(F) | `nf collapse` | `{op: collapse}` |
| "How related?" | R(X, Y) | `nf resonate @X @Y` | `{op: resonate, p1: X, p2: Y}` |

### 3.2 Queries

| Semantic | Algebraic | Shell | Canonical |
|----------|-----------|-------|-----------|
| "How clear?" | C(F) | `nf measure coherence` | `{query: coherence}` |
| "What insights?" | Attractors(F) | `nf attractor list` | `{query: attractors}` |
| "Current state" | F | `nf state` | `{query: state}` |
| "Show connections" | R matrix | `nf plot network` | `{query: network}` |

### 3.3 Field Management

| Semantic | Algebraic | Shell | Canonical |
|----------|-----------|-------|-----------|
| "Different angle" | F₂ ← init() | `nf field create` | `{op: field_create}` |
| "Switch focus" | select(F) | `nf field select` | `{op: field_select}` |
| "Connect views" | γ(F₁, F₂) | `nf couple` | `{op: couple}` |
| "Combine thinking" | sync | `nf sync` | `{op: sync}` |

---

## 4. Translation Commands

### 4.1 Explain Command

Translate and explain in all interfaces:

```
> nf translate "Focus strongly on security concerns"
[TRANSLATION]

  Semantic:   "Focus strongly on security concerns"
  Algebraic:  ι("security_concerns", 0.85)
  Shell:      nf inject "security_concerns" 0.85 --tags focus

  Explanation:
    Injects "security_concerns" pattern with high activation (0.85)
    The pattern will connect with related concepts through resonance
```

### 4.2 Convert Command

Convert between specific interfaces:

```
> nf convert --from algebraic --to semantic "Φ³(F)"
[CONVERT]
  Input (algebraic):  Φ³(F)
  Output (semantic):  "Let me think about this for 3 rounds"

> nf convert --from shell --to algebraic "nf amplify @security --factor 1.5"
[CONVERT]
  Input (shell):      nf amplify @security --factor 1.5
  Output (algebraic): α(@security, 1.5)

> nf convert --from semantic --to shell "What insights have emerged?"
[CONVERT]
  Input (semantic):   "What insights have emerged?"
  Output (shell):     nf attractor list
```

### 4.3 Parallel Display

Show operation in all interfaces simultaneously:

```
> nf parallel-view on

> nf inject "api_design" 0.8
[PARALLEL VIEW]
  ┌────────────────────────────────────────────────────────┐
  │ Shell:     nf inject "api_design" 0.8                  │
  │ Algebraic: ι("api_design", 0.8)                        │
  │ Semantic:  Added "api_design" to your thinking         │
  └────────────────────────────────────────────────────────┘
  [INJECT] Pattern 'api_design' added with activation 0.80
```

---

## 5. Semantic Parsing

### 5.1 Intent Recognition

```python
INTENT_PATTERNS = {
    # Injection intents
    r"(focus|concentrate|think about|consider|add)\s+(?:on\s+)?(.+)": "inject",
    r"(.+)\s+is\s+(important|relevant|key)": "inject_high",
    r"also\s+(?:consider|think about)\s+(.+)": "inject_moderate",

    # Amplification intents
    r"(emphasize|strengthen|boost|more)\s+(.+)": "amplify",
    r"(.+)\s+(?:is\s+)?(?:more\s+)?important": "amplify",
    r"(.+)\s+resonates": "amplify_resonance",

    # Attenuation intents
    r"(ignore|forget|de-emphasize|less)\s+(.+)": "attenuate",
    r"(.+)\s+(?:is\s+)?(?:less\s+)?(?:important|relevant)": "attenuate",
    r"(?:don't|do not)\s+(?:worry about|focus on)\s+(.+)": "attenuate",

    # Dynamics intents
    r"(think|process|develop|let .+ develop)": "cycle",
    r"(conclude|summarize|what's the insight)": "collapse",

    # Query intents
    r"(how|what)\s+(?:is|are)\s+(?:the\s+)?(clarity|coherence)": "query_coherence",
    r"what\s+(?:am I|are we)\s+thinking": "query_state",
    r"(?:show|what are)\s+(?:the\s+)?connections": "query_network",
    r"(?:what|any)\s+insights": "query_attractors",
}
```

### 5.2 Activation Mapping

```python
ACTIVATION_MODIFIERS = {
    # High activation (0.8-0.95)
    "strongly": 0.9,
    "really": 0.85,
    "very": 0.85,
    "highly": 0.9,
    "top priority": 0.95,
    "crucial": 0.92,
    "critical": 0.95,

    # Moderate activation (0.5-0.7)
    "also": 0.6,
    "consider": 0.6,
    "might": 0.55,
    "perhaps": 0.5,
    "maybe": 0.5,

    # Low activation (0.3-0.5)
    "background": 0.4,
    "minor": 0.35,
    "slight": 0.35,
}

def extract_activation(text):
    base = 0.7  # default
    for modifier, value in ACTIVATION_MODIFIERS.items():
        if modifier in text.lower():
            base = value
            break
    return base
```

---

## 6. Algebraic Parsing

### 6.1 Grammar

```
ALGEBRAIC_GRAMMAR:

expression := operation | query | assignment | compound

operation  := inject | amplify | attenuate | cycle | collapse | resonate
inject     := 'ι' '(' pattern ',' activation ')'
amplify    := 'α' '(' pattern ',' factor ')'
attenuate  := 'ν' '(' pattern ',' factor ')'
cycle      := 'Φ' [superscript] '(' field ')'
collapse   := 'κ' '(' field ')'
resonate   := 'R' '(' pattern ',' pattern ')'

query      := metric '(' field ')'
metric     := 'C' | 'E' | 'H' | 'S' | 'Attractors' | 'P'

assignment := variable '←' expression
compound   := expression ('∘' | ';') expression
conditional:= expression 'if' condition
```

### 6.2 Symbol Resolution

```python
ALGEBRAIC_SYMBOLS = {
    # Greek letters
    'ι': 'inject',    # iota
    'α': 'amplify',   # alpha (also used for amplification parameter)
    'ν': 'attenuate', # nu
    'Φ': 'cycle',     # Phi
    'κ': 'collapse',  # kappa
    'γ': 'couple',    # gamma (coupling)
    'λ': 'decay',     # lambda (decay parameter)
    'σ': 'bandwidth', # sigma (resonance bandwidth)
    'τ': 'threshold', # tau (attractor threshold)
    'ρ': 'route',     # rho

    # Metrics
    'C': 'coherence',
    'E': 'energy',
    'H': 'entropy',
    'S': 'stability',
    'R': 'resonance',

    # Sets
    'F': 'field',
    'P': 'patterns',
    'A*': 'attractor',
    'B': 'basin',
    'Σ': 'system',

    # Operators
    '∘': 'compose',
    '∈': 'in',
    '∀': 'forall',
    '∃': 'exists',
    '→': 'maps_to',
    '←': 'assign',
}
```

---

## 7. Bidirectional Translation

### 7.1 Semantic → Algebraic

```python
def semantic_to_algebraic(text):
    intent = parse_semantic_intent(text)

    if intent.type == "inject":
        activation = extract_activation(text)
        return f'ι("{intent.pattern}", {activation})'

    elif intent.type == "amplify":
        return f'α(@{intent.pattern}, 1.5)'

    elif intent.type == "cycle":
        count = extract_count(text) or 3
        return f'Φ{superscript(count)}(F)'

    elif intent.type == "query_coherence":
        return 'C(F)'

    # ... more mappings
```

### 7.2 Algebraic → Semantic

```python
def algebraic_to_semantic(expr):
    ast = parse_algebraic(expr)

    if ast.op == "inject":
        pattern = ast.args[0]
        activation = ast.args[1]
        intensity = activation_to_word(activation)
        return f'Focus {intensity} on {pattern}'

    elif ast.op == "cycle":
        n = ast.superscript or 1
        return f'Let me think about this for {n} round{"s" if n > 1 else ""}'

    elif ast.op == "coherence":
        return 'How clear is my thinking?'

    # ... more mappings

def activation_to_word(a):
    if a > 0.85: return "strongly"
    if a > 0.7: return ""  # default, no modifier
    if a > 0.5: return "also"
    return "in the background"
```

### 7.3 Shell ↔ Canonical

```python
def shell_to_canonical(command):
    tokens = tokenize(command)
    # nf inject "pattern" 0.8 --tags critical
    return {
        "operation": tokens[1],
        "arguments": parse_args(tokens[2:]),
        "source": {"interface": "shell", "original": command}
    }

def canonical_to_shell(ast):
    cmd = f"nf {ast['operation']}"
    for key, val in ast['arguments'].items():
        if key == 'pattern':
            cmd += f' "{val}"'
        elif key == 'activation':
            cmd += f' {val}'
        elif key == 'tags':
            cmd += f' --tags {",".join(val)}'
    return cmd
```

---

## 8. Context-Aware Translation

### 8.1 Ambiguity Resolution

```python
def resolve_ambiguity(text, context):
    # "It's important" - what is "it"?
    if "it" in text.lower():
        # Check recent patterns
        recent = context.recent_patterns[-1]
        text = text.replace("it", recent.name)

    # "More" - more what? cycles? activation?
    if text.strip().lower() == "more":
        if context.last_operation == "cycle":
            return {"type": "cycle", "count": context.last_count}
        elif context.last_operation == "inject":
            return {"type": "amplify", "pattern": context.last_pattern}

    return parse_semantic(text)
```

### 8.2 Contextual Defaults

```python
def apply_context_defaults(ast, context):
    # If no field specified, use active field
    if 'field' not in ast['arguments']:
        ast['arguments']['field'] = context.active_field

    # If no activation specified, infer from context
    if ast['operation'] == 'inject' and 'activation' not in ast['arguments']:
        if context.mode == 'exploration':
            ast['arguments']['activation'] = 0.6
        else:
            ast['arguments']['activation'] = 0.75

    return ast
```

---

## 9. Learning Mode

### 9.1 Dual Display

```
> nf interface learning
[INTERFACE] Learning mode: Shows all representations

🧠 > Focus on API design
[LEARNING VIEW]
  ┌─────────────────────────────────────────────────────────┐
  │ You said (semantic):                                    │
  │   "Focus on API design"                                 │
  │                                                         │
  │ Mathematically (algebraic):                             │
  │   ι("API_design", 0.7)                                  │
  │   "Inject pattern with activation 0.7"                  │
  │                                                         │
  │ As command (shell):                                     │
  │   nf inject "API_design" 0.7                            │
  │                                                         │
  │ What happens:                                           │
  │   A new pattern "API_design" is added to the field      │
  │   with moderate-high activation (0.7). It will          │
  │   connect with related patterns through resonance.      │
  └─────────────────────────────────────────────────────────┘
```

### 9.2 Translation Quiz

```
> nf learn quiz
[QUIZ] Translate this to algebraic notation:
  "Let me think about this for 5 rounds"

Your answer: _

> Φ⁵(F)
[CORRECT!]
  Φ = dynamics operator
  ⁵ = superscript for 5 iterations
  F = current field

Next question...
```

---

## 10. API Reference

```python
class TranslationEngine:
    """Translates between interface representations."""

    def __init__(self):
        self.semantic_parser = SemanticParser()
        self.algebraic_parser = AlgebraicParser()
        self.shell_parser = ShellParser()

    def to_canonical(self, text: str, source: str) -> CanonicalAST:
        """Parse input to canonical AST."""
        if source == "semantic":
            return self.semantic_parser.parse(text)
        elif source == "algebraic":
            return self.algebraic_parser.parse(text)
        elif source == "shell":
            return self.shell_parser.parse(text)

    def from_canonical(self, ast: CanonicalAST, target: str) -> str:
        """Generate target representation from AST."""
        if target == "semantic":
            return SemanticGenerator().generate(ast)
        elif target == "algebraic":
            return AlgebraicGenerator().generate(ast)
        elif target == "shell":
            return ShellGenerator().generate(ast)

    def translate(self, text: str, source: str, target: str) -> str:
        """Direct translation between interfaces."""
        ast = self.to_canonical(text, source)
        return self.from_canonical(ast, target)

    def explain(self, text: str, source: str) -> dict:
        """Generate explanations in all interfaces."""
        ast = self.to_canonical(text, source)
        return {
            "semantic": self.from_canonical(ast, "semantic"),
            "algebraic": self.from_canonical(ast, "algebraic"),
            "shell": self.from_canonical(ast, "shell"),
            "explanation": self.generate_explanation(ast)
        }
```

---

## 11. Quick Reference

### Translation Table

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| Focus on X | ι(X, 0.8) | `nf inject X 0.8` |
| Consider X | ι(X, 0.6) | `nf inject X 0.6` |
| Emphasize X | α(X, 1.5) | `nf amplify X` |
| De-emphasize X | ν(X, 0.5) | `nf attenuate X` |
| Think about this | Φ³(F) | `nf cycle 3` |
| Develop further | Φ*(F, C>τ) | `nf evolve` |
| Conclude | κ(F) | `nf collapse` |
| How related? | R(X, Y) | `nf resonate @X @Y` |
| How clear? | C(F) | `nf measure coherence` |
| What insights? | Attractors(F) | `nf attractor list` |
| Current state | F | `nf state` |
| Show connections | R matrix | `nf plot network` |

### Commands

```
nf interface <mode>           Switch interface mode
nf translate "<text>"         Show in all interfaces
nf convert --from X --to Y    Convert between specific interfaces
nf parallel-view on|off       Toggle parallel display
nf learn quiz                 Practice translations
```

---

## Related Documents

- `./algebraic.md` - Algebraic interface details
- `./semantic.md` - Semantic interface details
- `../commands/` - Shell command reference
