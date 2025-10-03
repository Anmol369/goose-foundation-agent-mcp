# Foundation Agent Quick Reference Card

**Version 1.0** | For Goose Users & Contributors

---

## What is Foundation Agent?

**A 4-phase reality validation system that helps you distinguish physics constraints from human conventions, enabling you to architect systems aligned with actual reality rather than imagined limitations.**

```
Your Request → Foundation Agent → Reality-Validated Foundation → Goose Implementation
```

---

## When to Use Foundation Agent

✅ **USE** for:
- Architecture decisions ("How should I structure this system?")
- Performance problems ("Why is this slow?")
- Scaling questions ("Can this handle 10× load?")
- Technology choices ("Microservices vs monolith?")
- Optimization decisions ("What should I optimize first?")

❌ **SKIP** for:
- 

**Rule of thumb:** If it affects >1000 lines or involves system-level decisions → Use Foundation Agent

---

## How to Invoke

### Basic Usage

```bash
goose session start --recipe foundation-agent

# Describe your system/problem
> "I need to architect a web application that handles 10,000 concurrent users
   with <100ms response times. Current architecture uses microservices but
   response time is 500ms. Database queries seem slow."
```

### Advanced Usage

```bash
# Specify which phase to focus on
goose session start --recipe foundation-agent --phase perception

# Use specific specialist
goose session start --recipe foundation-agent/specialists/architecture-agent

# Skip Foundation (direct to implementation)
goose session start  # default Goose behavior
```

---

## The 4 Phases (What to Expect)

### Phase 1: PERCEPTION (Reality Classifier)
**What it does:** Classifies constraints as PHYSICS vs CONSTRUCT
**Duration:** ~5 minutes
**Output:** Constraint classification map with confidence scores

**Example:**
```
Constraint: "Microservices architecture required"
Classification: CONVENTIONAL (δ = 0.80)
Rationale: 6 layers to physics, human invention, can be changed
Confidence: 0.92

Constraint: "Database queries >100ms"
Classification: PHYSICS (δ = 0.15)
Rationale: Disk I/O, memory hierarchy
Confidence: 0.88
Reference: physics-laws.yaml → memory_hierarchy.access_times
```

**Quality Gate:** δ_delusion < 0.25 (system on <25% assumptions)

### Phase 2: ARCHITECTURE (Pattern Mapper)
**What it does:** Creates physics-aligned minimal architecture
**Duration:** ~10 minutes
**Output:** Architectural blueprint with complexity reduction

**Example:**
```
Q-D-S-A-A Applied:
- QUESTIONED: "Why microservices?" → No valid reason
- DELETED: 8 microservices → 1 modular monolith
- SIMPLIFIED: 28 interfaces → 0
- ACCELERATED: Added Redis cache for hot queries
- AUTOMATED: Query performance monitoring

Complexity Reduction: 91% (12 components → 4)
Pattern Alignment: 100% (Flow Dynamics, Minimal Encoding)
```

**Quality Gate:** Complexity reduction > 40%, Pattern alignment > 75%

### Phase 3: EXECUTION (Flow Optimizer)
**What it does:** Identifies bottleneck, optimizes critical path
**Duration:** ~8 minutes
**Output:** Flow-optimized execution strategy

**Example:**
```
Bottleneck Analysis:
- Database queries: 800ms (80% of time) ← SINGULAR CONSTRAINT
- Network calls: 150ms (15% of time)
- CPU processing: 50ms (5% of time)

Critical Path: User → Monolith → Database → Response
Optimization: Index missing columns, connection pooling
Resource Allocation: 80% focus on database

Projected: 800ms → 70ms (11× faster)
E_flow: 0.81
```

**Quality Gate:** E_flow > 0.75, Singular constraint identified

### Phase 4: ADAPTATION (Learning Interface)
**What it does:** Calculates learning velocity, encodes knowledge
**Duration:** ~3 minutes
**Output:** Learning metrics and evolution pathway

**Example:**
```
Learning Velocity: dF/dt = 0.009 per cycle
Learning Rate: Lr = 1.8 (adapting 80% faster than environment)
Status: THRIVING

Knowledge Encoded:
- Pattern "database-missing-indexes" → validated-patterns.yaml
- Success rate: 0.94 (validated 23 times)
- Avg improvement: 2.8× velocity

Next Cycle Targets:
- Reduce δ_delusion from 0.18 to 0.15
- Increase F-score from 0.83 to 0.87
```

**Quality Gate:** Lr > 1.0 (learning faster than environment)

---

## Final Output (What You Get)

```json
{
  "foundation_metrics": {
    "F_score": 0.83,              // >0.8 = READY
    "delta_delusion": 0.17,       // <0.2 = GOOD
    "validation_ratio": 0.92
  },

  "executive_summary": {
    "status": "READY",
    "key_findings": [
      "Microservices architecture unnecessary (premature optimization)",
      "Database missing indexes (N+1 queries)",
      "External API calls should be cached"
    ],
    "critical_constraints": [
      {
        "constraint": "Database query latency",
        "type": "PHYSICS",
        "impact": "80% of response time"
      }
    ],
    "architectural_recommendations": [
      "Replace microservices with modular monolith",
      "Add database indexes on user_id, created_at",
      "Implement Redis caching layer for API responses"
    ],
    "optimization_priorities": [
      "1. Database indexing (highest impact)",
      "2. API response caching",
      "3. Connection pooling"
    ]
  },

  "process_agent_handoff": {
    "ready_for_handoff": true,
    "validated_constraints": { ... },
    "architectural_blueprints": { ... },
    "execution_strategy": { ... }
  }
}
```

---

## Key Metrics Explained

| Metric | Formula | Target | Meaning |
|--------|---------|--------|---------|
| **F-score** | (R_valid / R_total) × (1 - δ) | > 0.8 | Foundation effectiveness |
| **δ_delusion** | Weighted avg of construct confidence | < 0.2 | % operating on assumptions |
| **E_flow** | T_productive / T_total | > 0.75 | Flow efficiency |
| **Lr** | dF/dt / E_change | > 1.0 | Learning rate vs environment |
| **Cs** | Components × Interfaces | Minimize | System complexity |

### Interpreting F-score

- **F > 0.8:** READY - Foundation is solid, proceed with confidence
- **F ∈ [0.6, 0.8]:** NEEDS REFINEMENT - Some uncertainty remains
- **F < 0.6:** ABORT - Too much uncertainty, need more data

### Interpreting δ_delusion

- **δ < 0.2:** Excellent - Operating mostly on verified reality
- **δ ∈ [0.2, 0.4]:** Good - Some assumptions but manageable
- **δ > 0.4:** Poor - Too many unvalidated assumptions

---

## Common Patterns

### Pattern 1: "We need microservices"
**Classification:** CONVENTIONAL (δ = 0.80)
**Reality:** Architectural choice, not physics requirement
**Recommendation:** Start with modular monolith, extract services only when proven necessary

### Pattern 2: "Database is slow"
**Classification:** Usually CONSTRUCT (N+1 queries, missing indexes)
**Reality:** Rarely physics (disk I/O), usually architectural
**Recommendation:** Profile queries first, optimize before adding complexity

### Pattern 3: "Need more servers"
**Classification:** ECONOMIC (temporary) or ARCHITECTURAL (problem)
**Reality:** Often symptom, not cause
**Recommendation:** Identify singular bottleneck first

### Pattern 4: "Users want all features visible"
**Classification:** PHYSICS (cognitive limits)
**Reality:** Working memory = 7±2 items
**Recommendation:** Progressive disclosure, chunking, categorization

---

## Knowledge Bases

### physics-laws.yaml
Immutable constraints based on natural laws:
- Network latency (speed of light)
- Memory hierarchy (L1 cache → RAM → SSD → network)
- Human cognition (working memory, attention, response time perception)
- Thermodynamics (CPU heat, energy limits)
- Information theory (compression limits, error correction costs)

### human-constructs.yaml
Artificial patterns created by humans:
- Architectural patterns (microservices, MVC, layered)
- Design patterns (singleton, factory, observer)
- Frameworks (Rails, React, Django)
- Methodologies (Agile, TDD, DDD)
- Organizational (code review, branch protection)

### validated-patterns.yaml
Accumulated learning from real validations:
- Grows continuously as Foundation Agent validates systems
- Community contributions welcome
- Each pattern includes success rate, avg improvement, physics basis

---

## Troubleshooting

### "Quality gate failed: δ_delusion > 0.25"
**Cause:** Too many constraints classified with low confidence
**Fix:**
- Provide more context about your system
- Add measurements/profiling data
- Break down compound constraints

### "No singular constraint identified"
**Cause:** Multiple bottlenecks with similar impact
**Fix:**
- Profile more granularly
- Check if constraints are coupled (must address together)
- Measurement data may be insufficient

### "Complexity increased instead of decreased"
**Cause:** Architecture Agent added components rather than eliminating
**Fix:**
- Review Q-D-S-A-A application
- Apply DELETE more aggressively
- Question whether constructs are truly necessary

### "Learning rate < 1.0 (falling behind)"
**Cause:** Environment changing faster than system adapts
**Fix:**
- Increase validation frequency
- Focus on meta-learning (improve improvement process)
- Reduce environmental change rate (stabilize inputs)

---

## Contributing

### Add Physics Constraint
```yaml
# knowledge/physics-laws.yaml
network_physics:
  your_constraint:
    value: "quantitative measurement"
    source: "cite research paper or spec"
    implications:
      - "practical meaning for developers"
    workarounds:
      - "how to work WITH this constraint"
```

### Add Validated Pattern
```yaml
# knowledge/validated-patterns.yaml
- pattern_id: "your-pattern-id"
  constraint: "clear description"
  classification: "PHYSICS | CONSTRUCT"
  validation_method: "how you validated this"
  confidence: 0.92
  success_rate: 0.88
  avg_improvement: "2.5× velocity"
  physics_basis: "which law/principle"
```

---

## FAQ

**Q: How long does Foundation Agent take?**
A: ~25-30 minutes for full 4-phase analysis. Can run individual phases in ~5-10 min.

**Q: Can I skip phases?**
A: Yes, but not recommended. Each phase builds on previous. Skipping = lower F-score.

**Q: What if I disagree with classification?**
A: Provide evidence (measurements, profiling data). Foundation Agent updates confidence based on data.

**Q: Does this work for all domains?**
A: Currently optimized for web/distributed systems. Mobile, embedded, ML domains have domain-specific physics-laws.yaml coming soon.

**Q: Can I use my own knowledge bases?**
A: Yes! Fork physics-laws.yaml and add domain-specific constraints. Contribute back to community.

**Q: How accurate is Foundation Agent?**
A: Cross-validation shows >85% accuracy on constraint classification. Improves with more validated-patterns.

**Q: What models does it use?**
A: Default: claude-sonnet-4. Can configure per-specialist agent (e.g., haiku for speed, opus for accuracy).

---

## Quick Commands Cheat Sheet

```bash
# Full 4-phase analysis
goose session start --recipe foundation-agent

# Just classification (Phase 1)
goose session start --recipe foundation-agent/specialists/perception-agent

# Just architecture (Phase 2, requires Phase 1 output)
goose session start --recipe foundation-agent/specialists/architecture-agent

# Check knowledge bases
goose foundation list-physics-laws
goose foundation list-constructs
goose foundation list-validated-patterns

# View metrics
goose foundation metrics --session <id>

# Contribute pattern
goose foundation contribute-pattern --file my-pattern.yaml
```

---

## Resources

- **Full Documentation:** `FOUNDATION_AGENT_IMPLEMENTATION_STRATEGY.md`
- **Concepts:** [Foundation Agent Philosophy]
- **Examples:** `recipes/foundation-agent/examples/`
- **Community:** [Discord/Slack]
- **Issues:** [GitHub Issues]

---

**Remember:** Foundation Agent helps you validate reality BEFORE architecting solutions. The goal is not perfect architecture, but **physics-aligned architecture** that operates on verified constraints rather than imagined limitations.

**"Measurement before assumption. Reality before convention. Physics before opinion."**
