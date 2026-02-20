# Semantic Interface

Natural language interface for intuitive, conversational interaction with neural fields.

---

## 1. Overview

The semantic interface allows natural language interaction with NFOS, translating human-readable descriptions into field operations and presenting results in intuitive terms.

**Benefits:**
- Intuitive, accessible interaction
- Descriptive operation names
- Contextual explanations
- Metaphor-based understanding

---

## 2. Interaction Modes

### 2.1 Conversational Mode

```
> nf interface semantic
[INTERFACE] Switched to semantic/conversational mode

🧠 > _
```

### 2.2 Natural Language Commands

```
🧠 > Start thinking about security vulnerabilities
[FIELD] Created focus on "security vulnerabilities"
  Initial patterns: vulnerability, threat, risk
  Field is ready for exploration

🧠 > Add the concept of SQL injection with high importance
[INJECT] Added "SQL injection" to your thinking
  Importance: high (0.85)
  Related concepts will naturally connect

🧠 > What patterns are emerging?
[STATE] Current thinking patterns:
  • SQL injection (strong focus)
  • Input validation (connecting)
  • User data handling (emerging)

  An insight is forming around "input security"
```

---

## 3. Semantic Operations

### 3.1 Creating Focus

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| "Start thinking about X" | F ← init(X) | `nf init X` |
| "Focus on X" | ι(X, 0.8) | `nf inject X 0.8` |
| "Consider X" | ι(X, 0.6) | `nf inject X 0.6` |
| "Add X to my thinking" | ι(X, 0.7) | `nf inject X 0.7` |

**Examples:**
```
🧠 > Start thinking about API design
[FIELD] Created thinking space for "API design"

🧠 > Focus strongly on REST principles
[INJECT] REST principles now has your attention (high importance)

🧠 > Also consider GraphQL as an alternative
[INJECT] GraphQL added as consideration (moderate importance)

🧠 > What about versioning?
[INJECT] Versioning added to your thinking
```

### 3.2 Strengthening Ideas

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| "Emphasize X" | α(X, 1.5) | `nf amplify X` |
| "X is more important" | α(X, 1.3) | `nf amplify X --factor 1.3` |
| "Focus more on X" | α(X, 1.2) | `nf amplify X --factor 1.2` |
| "X resonates with me" | α(X, R) | `nf amplify X` |

**Examples:**
```
🧠 > Security is really important here
[AMPLIFY] Security concerns strengthened
  Now: strong focus (was: moderate)
  Related patterns also gaining attention

🧠 > The performance aspect resonates with what I'm seeing
[AMPLIFY] Performance boosted by resonance
  Connected to: caching, optimization, latency
```

### 3.3 De-emphasizing Ideas

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| "X is less relevant" | ν(X, 0.5) | `nf attenuate X` |
| "Ignore X for now" | ν(X, 0.8) | `nf attenuate X --factor 0.8` |
| "X isn't the focus" | ν(X, 0.3) | `nf attenuate X --factor 0.3` |
| "Let X fade" | natural decay | (automatic) |

**Examples:**
```
🧠 > The logging concern isn't as important
[ATTENUATE] Logging de-emphasized
  Now: background consideration
  More focus available for other patterns

🧠 > Let's not worry about backwards compatibility right now
[ATTENUATE] Backwards compatibility fading
  Will remain if it connects to other ideas
```

### 3.4 Exploring Connections

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| "How does X relate to Y?" | R(X, Y) | `nf resonate @X @Y` |
| "What connects to X?" | {p : R(p,X) > 0.5} | `nf resonate @X --all` |
| "Show me the connections" | plot network | `nf plot network` |
| "What's related?" | resonance analysis | `nf measure` |

**Examples:**
```
🧠 > How does security relate to user input?
[RESONANCE] Strong connection found (0.82)
  Security and user input are closely linked
  Common themes: validation, trust boundary, sanitization

🧠 > What connects to the authentication concept?
[CONNECTIONS] Authentication links to:
  • Session management (strong)
  • User identity (strong)
  • Authorization (moderate)
  • Encryption (moderate)
```

### 3.5 Letting Ideas Develop

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| "Let me think about this" | Φⁿ(F) | `nf cycle n` |
| "Develop these ideas" | Φ*(F, C>τ) | `nf evolve` |
| "What emerges?" | attractor detection | `nf attractor list` |
| "Give it time to settle" | evolve to stability | `nf evolve --target stable` |

**Examples:**
```
🧠 > Let me think about this for a moment
[THINKING] Processing your ideas...
  Round 1: Connections forming between security concepts
  Round 2: Input validation cluster strengthening
  Round 3: An insight is crystallizing...

  Insight emerged: "Input Security Pattern"
  Core ideas: SQL injection, input validation, user data

🧠 > Develop these ideas further
[EVOLVE] Letting ideas mature...
  Your thinking is becoming more coherent
  Clarity: 72% → 85%

  Strong insight: Input validation is central to security
```

### 3.6 Getting Conclusions

| Semantic | Algebraic | Shell |
|----------|-----------|-------|
| "What's the conclusion?" | κ(F) | `nf collapse` |
| "Summarize my thinking" | κ(F) | `nf collapse` |
| "What's the main insight?" | dominant attractor | `nf collapse --strategy coherence` |
| "What should I focus on?" | recommendations | `nf collapse` |

**Examples:**
```
🧠 > What's the main insight here?
[INSIGHT] Your thinking has crystallized around:

  "Security-First Input Handling"

  Key points:
  • All user input should be validated
  • SQL injection is a primary concern
  • Parameterized queries are essential

  Confidence: 88%

🧠 > Summarize what I've been thinking about
[SUMMARY] Your exploration covered:

  Main focus: API Security Design

  Core insights:
  1. Input validation is foundational
  2. Authentication needs careful design
  3. REST and security can coexist well

  Secondary considerations:
  • Performance trade-offs
  • Logging for audit trails
```

---

## 4. Conversational Queries

### 4.1 State Queries

```
🧠 > What am I thinking about?
[STATE] Your current focus:

  Strong attention:
  • Security vulnerabilities
  • Input validation

  Moderate attention:
  • SQL injection patterns
  • Authentication

  Background:
  • Logging
  • Performance

🧠 > How clear is my thinking?
[COHERENCE] Your thinking is fairly coherent (74%)
  Main ideas are connecting well
  Some peripheral concepts still loose

🧠 > What insights have emerged?
[ATTRACTORS] Insights in your thinking:

  1. "Security Input Cluster" (strong, 82%)
     Ideas: SQL injection, input validation, sanitization

  2. "Auth Pattern" (forming, 65%)
     Ideas: authentication, session, identity

🧠 > Is my thinking stable?
[STABILITY] Your thinking is settling (stability: 78%)
  The main insight is solid
  Some ideas still in flux
```

### 4.2 Relationship Queries

```
🧠 > What's most connected to security?
[CONNECTIONS] Ideas most related to security:
  1. Input validation (92% connected)
  2. SQL injection (88% connected)
  3. Authentication (75% connected)
  4. User trust (68% connected)

🧠 > Are there any tensions in my thinking?
[TENSIONS] Some ideas don't align well:
  • Performance vs Security (tension: 35%)
    Fast execution may conflict with thorough validation

  • Simplicity vs Completeness (tension: 28%)
    Comprehensive security adds complexity

🧠 > What ideas are clustering together?
[CLUSTERS] Natural groupings:

  Cluster 1: "Input Security"
    SQL injection, validation, sanitization

  Cluster 2: "Identity & Access"
    Authentication, authorization, sessions

  Cluster 3: "Operational"
    Logging, monitoring, performance
```

### 4.3 Guidance Queries

```
🧠 > What should I think about next?
[SUGGESTION] Based on your patterns:
  Consider exploring "output encoding"
  It connects to your security focus but isn't yet addressed
  Resonance potential: 72%

🧠 > Am I missing anything?
[GAPS] Potential blind spots:
  • Error handling (related but not addressed)
  • Rate limiting (security-adjacent)
  • Data encryption at rest

  Would you like to explore any of these?

🧠 > How should I proceed?
[GUIDANCE] Your thinking has good coherence (78%)

  Options:
  1. Let it crystallize more (2-3 more rounds)
  2. Extract insights now (good confidence)
  3. Explore gaps (error handling, encryption)

  What feels right?
```

---

## 5. Metaphor-Based Explanations

### 5.1 Field as Mind

```
🧠 > Explain what's happening
[EXPLANATION]

  Think of this as your mind focusing on a problem:

  • Ideas you've introduced are like thoughts entering awareness
  • Strong ideas (high activation) are what you're actively thinking about
  • Connections form naturally between related thoughts
  • Over time, your mind finds patterns and insights emerge
  • The "insight" (attractor) is the conclusion your thinking settles on
```

### 5.2 Resonance as Harmony

```
🧠 > How do resonance and connection work?
[EXPLANATION]

  Resonance is like harmony between musical notes:

  • Ideas that "sound good together" resonate strongly
  • Resonant ideas reinforce each other
  • Dissonant ideas create tension
  • Your thinking naturally seeks harmony (coherence)
  • Clusters of harmonious ideas become insights
```

### 5.3 Attractors as Insights

```
🧠 > What are these "insights" technically?
[EXPLANATION]

  An insight (attractor) is like a mental resting place:

  • It's a stable configuration of connected ideas
  • Your thinking naturally "falls into" insights
  • Like a ball settling into a valley
  • The deeper the valley, the stronger the insight
  • Multiple insights can coexist (multiple valleys)
```

---

## 6. Multi-Perspective Thinking

### 6.1 Creating Perspectives

```
🧠 > Let me think about this from different angles
[PERSPECTIVES] Creating multiple viewpoints:
  • Technical perspective (created)
  • User perspective (created)
  • Business perspective (created)

  Each will develop independently then combine

🧠 > From a technical standpoint, security is paramount
[TECHNICAL] Technical view updated:
  Security given high importance

🧠 > Users care most about ease of use
[USER] User view updated:
  Usability given high importance

🧠 > The business needs it done quickly
[BUSINESS] Business view updated:
  Timeline given high importance
```

### 6.2 Synthesizing Perspectives

```
🧠 > How do these perspectives align?
[SYNTHESIS] Comparing viewpoints:

  Agreement (high harmony):
  • Feature value - all perspectives see importance
  • Core functionality - aligned on basics

  Tension (low harmony):
  • Security vs Speed - technical wants thorough, business wants fast
  • Simplicity vs Features - user wants simple, technical sees complexity

🧠 > Find a balanced conclusion
[SYNTHESIS] Balancing perspectives...

  Integrated insight:
  "Phased delivery with security-first approach"

  Satisfies:
  • Technical: Security addressed early
  • User: Core features delivered simply
  • Business: Phased approach meets timeline

  Trade-offs acknowledged:
  • Some features delayed to Phase 2
  • Security adds 15% to Phase 1
```

---

## 7. Semantic Mode Commands

### 7.1 Quick Commands

Even in semantic mode, shortcuts are available:

```
🧠 > /focus security     (inject with high activation)
🧠 > /consider logging   (inject with moderate activation)
🧠 > /think 5            (run 5 cycles)
🧠 > /conclude           (collapse)
🧠 > /show               (state)
🧠 > /connect            (show network)
```

### 7.2 Mode Preferences

```
🧠 > /verbose on
[PREFERENCE] Detailed explanations enabled

🧠 > /metaphors off
[PREFERENCE] Technical language preferred

🧠 > /guidance on
[PREFERENCE] Proactive suggestions enabled
```

---

## 8. Example Session

### Complete Semantic Workflow

```
> nf interface semantic
[INTERFACE] Semantic mode active

🧠 > I want to think through designing a new authentication system
[FIELD] Created thinking space: "Authentication System Design"
  Starting with: authentication, system, design
  Ready to explore

🧠 > Security is my top priority
[INJECT] Security added as top priority
  Your focus is now security-oriented

🧠 > I need to support OAuth and traditional login
[INJECT] Added to your thinking:
  • OAuth integration
  • Traditional username/password

  These are connecting to your security focus

🧠 > Session management is important too
[INJECT] Session management added
  Connecting to: authentication, security

🧠 > Let me think this through
[THINKING] Processing your ideas...

  Round 1: OAuth and traditional auth finding common ground
  Round 2: Security concerns linking everything
  Round 3: Session management connecting to both approaches
  Round 4: An insight is forming...
  Round 5: Insight crystallized

  Insight emerged: "Unified Auth Layer"
  Core: OAuth, sessions, security validation
  Coherence: 78%

🧠 > What's the relationship between OAuth and sessions?
[RESONANCE] Strong connection (0.75)
  Both manage user identity continuity
  OAuth provides tokens, sessions track state
  They complement each other

🧠 > Are there any tensions?
[TENSIONS] Minor tension detected:
  • Token-based (OAuth) vs Session-based (traditional)
    Different state management paradigms
    Tension: 32%

  Suggestion: Consider JWT for unified approach

🧠 > Good idea, add JWT to my thinking
[INJECT] JWT added
  Immediately resonating with both OAuth and sessions
  Potential bridge concept

🧠 > Let this develop more
[EVOLVE] Developing ideas...
  JWT connecting the approaches
  Coherence rising: 78% → 85%
  Insight strengthening

🧠 > What's my conclusion?
[INSIGHT] Your thinking has crystallized:

  "JWT-Based Unified Authentication"

  Architecture:
  • OAuth for external identity providers
  • Traditional login converts to JWT
  • Sessions backed by JWT validation
  • Single security validation layer

  Key benefits:
  • Consistent security model
  • Stateless scalability
  • Flexible identity sources

  Confidence: 85%

  Recommendation: Proceed with this design
```

---

## 9. Configuration

### 9.1 Semantic Settings

```yaml
interface:
  mode: semantic
  settings:
    language_style: conversational  # formal, conversational, casual
    explanation_level: detailed     # brief, moderate, detailed
    use_metaphors: true
    proactive_guidance: true
    emotion_indicators: true        # 🧠, ✨, etc.
    default_importance:
      focus: 0.85
      consider: 0.65
      background: 0.45
```

### 9.2 Personality Tuning

```
🧠 > /style formal
[STYLE] Switching to formal academic language

🧠 > /style casual
[STYLE] Switching to casual conversational tone

🧠 > /style default
[STYLE] Using balanced conversational style
```

---

## 10. API Reference

```python
class SemanticInterface:
    """Natural language interface for NFOS."""

    def parse_intent(self, text: str) -> Intent:
        """Parse natural language into operation intent."""

    def execute_semantic(self, text: str) -> SemanticResult:
        """Execute natural language command."""

    def explain_state(self, style: str = "metaphor") -> str:
        """Generate human-readable state explanation."""

    def suggest_next(self) -> List[Suggestion]:
        """Generate contextual suggestions."""

    def format_response(self, result: Any, verbose: bool = True) -> str:
        """Format operation result for human reading."""
```

---

## Related Documents

- `./algebraic.md` - Mathematical interface
- `./translation.md` - Interface translation
- `../commands/` - Shell command reference
