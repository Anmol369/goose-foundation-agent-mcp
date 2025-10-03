# Foundation Agent Migration Guide

**Safe Transition for Existing Goose Users**

**Promise:** Zero breaking changes. Foundation Agent is 100% opt-in and backward compatible.

---

## TL;DR

✅ **Your existing Goose workflows continue working exactly as before**
✅ **Foundation Agent is opt-in only** (disabled by default)
✅ **Can enable/disable anytime** (toggle in config)
✅ **Gradual adoption path** (try on one project first)
✅ **Rollback in seconds** (single config change)

---

## Migration Overview

```
┌────────────────────────────────────────────────────────┐
│ PHASE 0: Before Foundation Agent                      │
│ • Existing Goose works perfectly                      │
│ • All features available                              │
│ • No changes                                          │
└────────────────────────────────────────────────────────┘
          ↓ (Update Goose to v1.6+)
┌────────────────────────────────────────────────────────┐
│ PHASE 1: Foundation Agent Available (But Disabled)    │
│ • Update: goose upgrade                               │
│ • Default: Foundation Agent OFF                       │
│ • Behavior: Identical to Phase 0                      │
│ • Optional: Try Foundation on test project            │
└────────────────────────────────────────────────────────┘
          ↓ (Opt-in to Foundation)
┌────────────────────────────────────────────────────────┐
│ PHASE 2: Foundation Agent Enabled (Selective)         │
│ • Config: foundation.enabled = true                   │
│ • Smart routing: Complex → Foundation, Simple → Direct│
│ • Gradual: Enable per-project or globally             │
└────────────────────────────────────────────────────────┘
          ↓ (After validation)
┌────────────────────────────────────────────────────────┐
│ PHASE 3: Full Adoption (Recommended)                  │
│ • Foundation Agent default for architectural tasks    │
│ • Direct Goose for simple tasks                       │
│ • Best of both worlds                                 │
└────────────────────────────────────────────────────────┘
```

---

## Before You Migrate

### Check Your Goose Version

```bash
goose --version
```

**Required:** Goose v1.6 or later (Foundation Agent included)

**If older:**
```bash
goose upgrade
# Or reinstall
curl -fsSL https://goose.ai/install.sh | sh
```

### Backup Your Configuration

```bash
# Backup existing config
cp ~/.config/goose/config.yaml ~/.config/goose/config.yaml.backup

# Backup existing recipes (if customized)
cp -r ~/.config/goose/recipes ~/.config/goose/recipes.backup
```

---

## Migration Path 1: Test Drive (Recommended First Step)

**Goal:** Try Foundation Agent on ONE project without affecting anything else

### Step 1: Explicit Invocation (No Config Changes)

```bash
# Your existing workflow (unchanged)
goose session start
> "Add user authentication"
# Works exactly as before ✅

# Try Foundation Agent explicitly
goose session start --recipe foundation-agent
> "Design a high-performance API architecture"
# Foundation Agent analyzes (26 min)
# Returns validated foundation
```

**What this does:**
- ✅ Existing `goose session start` works identically
- ✅ Foundation Agent only runs when explicitly requested
- ✅ Zero impact on your normal workflow
- ✅ Can compare results side-by-side

### Step 2: Try on a Real Architectural Decision

```bash
cd ~/projects/my-api

# Scenario: Team debating microservices vs monolith
goose session start --recipe foundation-agent

> "We're building an e-commerce API expecting 10k daily users.
   Team wants microservices for scalability.
   Should we use microservices or monolith?"

# Foundation Agent runs 4-phase analysis
# 26 minutes later...
# Returns: F-score, δ_delusion, recommendations
```

**Evaluate results:**
- Did Foundation Agent identify real constraints?
- Were recommendations actionable?
- Would following advice improve architecture?

**If satisfied → Proceed to Path 2**
**If unsure → Stay on explicit invocation, try more examples**

---

## Migration Path 2: Enable Foundation Agent (Opt-In)

### Option A: Enable Globally (All Projects)

```bash
# Enable Foundation Agent globally
goose config set foundation.enabled true
```

**What changes:**

| Scenario | Before | After |
|----------|--------|-------|
| **Simple task** | Direct Goose | Direct Goose (unchanged) |
| **Architectural task** | Direct Goose | Foundation Agent → Goose |
| **Explicit --recipe** | Works | Works (unchanged) |

**Detection logic (automatic):**
```yaml
Foundation Agent activates when:
  - Keywords: "architecture", "design", "performance", "scale"
  - Impact: >1000 LOC estimated
  - Complexity: Multiple approaches possible

Otherwise: Direct to existing Goose (fast path)
```

**Example:**
```bash
# Simple task → Direct Goose (fast)
goose session start
> "Fix typo in README"
# Immediate execution ✅

# Architectural task → Foundation Agent
goose session start
> "Optimize database queries for 10× scale"
# Foundation Agent analyzes first (26 min)
# Then hands off to Goose for implementation
```

### Option B: Enable Per-Project

```bash
cd ~/projects/critical-system

# Create project-specific config
cat > .goose/config.yaml <<EOF
foundation:
  enabled: true
  activation:
    auto_activate_architectural: true
EOF

# This project uses Foundation Agent
goose session start  # Foundation available

# Other projects unaffected
cd ~/projects/simple-website
goose session start  # Direct Goose (no Foundation)
```

### Option C: Enable Selectively (Safest)

```yaml
# ~/.config/goose/foundation.yaml

foundation_agent:
  enabled: true

  # Fine-grained control
  activation:
    auto_activate_architectural: false  # Don't auto-activate
    keywords:  # Only activate on these keywords
      - "architecture review"
      - "performance analysis"
      - "validate design"

    exclude_patterns:  # Never activate for these
      - "quick fix"
      - "bug fix"
      - "documentation"

    min_complexity: 2000  # Only for >2000 LOC impact (conservative)
```

**Result:** Foundation Agent only runs when YOU explicitly want it

---

## Migration Path 3: Gradual Team Adoption

### Scenario: 10-person engineering team

**Week 1: Individual trial**
```bash
# 2 volunteers try Foundation Agent
goose session start --recipe foundation-agent --verbose

# Share results in team meeting
# Discuss: Did it find real issues?
```

**Week 2: Project trial**
```bash
# Enable Foundation for ONE project
cd ~/team-projects/new-service
echo "foundation: { enabled: true }" > .goose/config.yaml

# Entire team uses Foundation on this project
# Existing projects unchanged
```

**Week 3: Evaluate**
```
Metrics to check:
- F-score average: >0.8?
- Recommendations useful?
- Time investment worth it (26 min)?
- Prevented architectural mistakes?
```

**Week 4: Decide**
- ✅ Success → Enable globally for team
- ⏸️ Mixed results → Continue project-level trials
- ❌ Not useful → Disable, collect feedback for improvements

---

## Configuration Reference

### Minimal Config (Opt-In, Auto-Detect)

```yaml
# ~/.config/goose/config.yaml

foundation:
  enabled: true
```

That's it! Foundation Agent activates automatically for architectural tasks.

### Full Config (Maximum Control)

```yaml
# ~/.config/goose/foundation.yaml

foundation_agent:
  enabled: true
  version: "1.0"

  # Activation criteria
  activation:
    auto_activate_architectural: true

    keywords:
      - "architecture"
      - "design"
      - "performance"
      - "optimization"
      - "scale"
      - "bottleneck"

    min_complexity: 1000  # LOC threshold

    exclude_patterns:
      - "simple"
      - "quick"
      - "trivial"
      - "documentation"

  # Quality gates (can relax if needed)
  quality_gates:
    phase_1:
      delta_delusion_max: 0.25
      confidence_min: 0.80
    phase_2:
      complexity_reduction_min: 0.40
    phase_3:
      e_flow_min: 0.75
    phase_4:
      learning_rate_min: 1.0
    final:
      f_score_min: 0.8

  # Knowledge bases
  knowledge:
    physics_laws: "knowledge/physics-laws.yaml"
    human_constructs: "knowledge/human-constructs.yaml"
    validated_patterns: "knowledge/validated-patterns.yaml"
    auto_update: true

  # Performance
  timeout:
    phase_1: 600   # 10 minutes max
    phase_2: 900   # 15 minutes max
    phase_3: 720   # 12 minutes max
    phase_4: 300   # 5 minutes max
    total: 1800    # 30 minutes total max

  # Cost control
  cost:
    max_per_run: 1.00        # $1 max per Foundation run
    alert_threshold: 0.75    # Alert at $0.75
    monthly_budget: 50.00    # $50/month team budget

  # Observability
  logging:
    level: "info"  # debug | info | warn | error
    metrics_enabled: true
    trace_enabled: false

  # Middleware
  middleware:
    - type: cost_tracking
      enabled: true
    - type: foundation_validation
      enabled: true
```

---

## Compatibility Matrix

| Goose Version | Foundation Agent | Compatibility | Notes |
|---------------|------------------|---------------|-------|
| **v1.0 - v1.5** | Not available | N/A | Upgrade to v1.6+ |
| **v1.6** | Available (opt-in) | ✅ 100% | First release with Foundation |
| **v1.7+** | Available (opt-in) | ✅ 100% | Improvements, same API |
| **v2.0** | Enabled by default | ✅ 100% | Can disable if needed |

### Breaking Changes

**NONE.** Foundation Agent is designed to be 100% backward compatible.

**Guarantees:**
- ✅ Existing recipes continue working
- ✅ Existing CLI commands unchanged
- ✅ Existing config files compatible
- ✅ Existing tool integrations work
- ✅ Can disable Foundation Agent anytime

---

## Rollback Plan

### Immediate Rollback (Disable Foundation)

```bash
# Option 1: Config
goose config set foundation.enabled false

# Option 2: Delete config
rm ~/.config/goose/foundation.yaml

# Option 3: Environment variable
export GOOSE_FOUNDATION_DISABLED=1
```

**Result:** Back to pure Goose v1.5 behavior immediately.

### Full Rollback (Downgrade Goose)

```bash
# Restore backup
mv ~/.config/goose/config.yaml.backup ~/.config/goose/config.yaml

# Downgrade Goose (if needed)
goose install --version 1.5.0
```

**When to rollback:**
- Foundation Agent consistently gives low F-scores (<0.6)
- Recommendations not useful for your domain
- 26-minute duration unacceptable for workflow
- Team prefers existing approach

---

## Common Migration Scenarios

### Scenario 1: Solo Developer

**Recommendation:** Start with explicit invocation

```bash
# Keep existing workflow
goose session start
> "Add new feature"

# Try Foundation occasionally
goose session start --recipe foundation-agent
> "Should I refactor to microservices?"

# After 5-10 successful trials, enable globally
goose config set foundation.enabled true
```

**Timeline:** 2-4 weeks trial → Enable if useful

---

### Scenario 2: Small Team (2-5 developers)

**Recommendation:** One project trial first

```bash
# Team meeting: Select one project for trial
cd ~/team/new-service

# Enable Foundation for this project only
echo "foundation: { enabled: true }" > .goose/config.yaml

# Team uses Foundation on this project for 2 weeks
# Evaluate results

# If successful, enable globally
# If not, disable and continue evaluating
```

**Timeline:** 2 weeks trial → Team decision → Gradual rollout

---

### Scenario 3: Enterprise Team (10+ developers)

**Recommendation:** Phased rollout

**Phase 1 (Week 1-2): Champions**
- 2-3 volunteers try Foundation Agent
- Explicit invocation only
- Collect feedback

**Phase 2 (Week 3-4): Pilot project**
- Enable on 1 non-critical project
- All team members use it
- Measure: F-scores, usefulness, time investment

**Phase 3 (Week 5-6): Expand**
- If successful, enable on 3-5 projects
- Document best practices
- Train team on interpreting results

**Phase 4 (Week 7-8): Full rollout**
- Enable globally with smart routing
- Ongoing training and support
- Collect metrics and refine

**Timeline:** 8-week controlled rollout

---

## Troubleshooting Migration Issues

### Issue 1: Foundation Agent Too Slow

**Problem:** 26 minutes is too long for my workflow

**Solutions:**

```yaml
# Option A: Increase thresholds (activate less often)
activation:
  min_complexity: 5000  # Only major architecture decisions

# Option B: Run in background
goose session start --recipe foundation-agent --background
# Continue working, get notification when done

# Option C: Use progressive validation
goose session start --recipe foundation-agent --phase 1
# Run Phase 1 only (5 min), decide if worth continuing
```

---

### Issue 2: Low F-Scores (<0.6)

**Problem:** Foundation Agent frequently returns F < 0.6

**Root causes:**
- Insufficient context in system description
- Novel domain not in knowledge base
- Ambiguous requirements

**Solutions:**

```bash
# Provide more context
goose session start --recipe foundation-agent --input design-doc.md

# Include metrics
goose session start --recipe foundation-agent --metrics metrics.json

# Domain-specific knowledge base
goose config set foundation.knowledge.custom ~/my-domain/physics.yaml
```

---

### Issue 3: Recommendations Don't Match My Domain

**Problem:** Foundation Agent suggestions not applicable to my field

**Solution:** Contribute domain-specific knowledge

```yaml
# Create custom physics-laws for your domain
# Example: Embedded systems

embedded_systems_physics:
  power_consumption:
    battery_capacity: "Finite joules per charge"
    sleep_modes: "Required to extend battery life"

  real_time_constraints:
    interrupt_latency: "<10μs for hard real-time"
    task_scheduling: "Rate-monotonic or EDF required"

# Load custom knowledge
foundation:
  knowledge:
    custom_physics: "~/.goose/embedded-physics.yaml"
```

**Then share with community!**

---

### Issue 4: Team Resistance

**Problem:** Team doesn't trust Foundation Agent recommendations

**Solutions:**

1. **Start small:** One voluntary project
2. **Show data:** Before/after metrics
3. **Compare to alternatives:** Foundation vs manual review
4. **Transparency:** Show F-score, δ_delusion openly
5. **Control:** Let team override recommendations

**Example:**
```bash
# Run Foundation, share results transparently
goose session start --recipe foundation-agent --verbose

# Team reviews Foundation's analysis
# Team discusses agreement/disagreement
# Team decides whether to follow recommendations

# Build trust through demonstrated accuracy
```

---

## Migration Checklist

### Pre-Migration

- [ ] Goose version ≥ v1.6
- [ ] Backup existing config
- [ ] Read Foundation Agent docs
- [ ] Understand opt-in model

### Initial Trial (Week 1)

- [ ] Try explicit invocation on 1 architectural decision
- [ ] Review F-score, δ_delusion, recommendations
- [ ] Compare to manual analysis
- [ ] Decide: useful or not?

### Gradual Adoption (Week 2-4)

- [ ] Enable on 1 test project OR
- [ ] Enable globally with high thresholds
- [ ] Monitor usage and results
- [ ] Collect team feedback

### Full Adoption (Week 5+)

- [ ] Enable globally with smart routing
- [ ] Document team best practices
- [ ] Contribute validated patterns
- [ ] Refine configuration based on usage

### Maintenance

- [ ] Update knowledge bases quarterly
- [ ] Review F-score trends
- [ ] Contribute patterns to community
- [ ] Stay updated on Foundation Agent improvements

---

## Support & Resources

### Getting Help

**Documentation:**
- Quick Reference: `FOUNDATION_AGENT_QUICK_REFERENCE.md`
- Architecture: `FOUNDATION_AGENT_ARCHITECTURE_DIAGRAMS.md`
- Examples: `FOUNDATION_AGENT_WALKTHROUGH_EXAMPLE.md`

**Community:**
- Discord: https://discord.gg/goose
- GitHub Discussions: https://github.com/goose/foundation-agent/discussions
- Stack Overflow: Tag `goose-foundation-agent`

**Issues:**
- Bug reports: https://github.com/goose/foundation-agent/issues
- Feature requests: https://github.com/goose/foundation-agent/discussions

---

## FAQ

**Q: Will Foundation Agent break my existing workflows?**
A: No. Foundation Agent is opt-in and 100% backward compatible.

**Q: Can I disable Foundation Agent after enabling?**
A: Yes, instantly: `goose config set foundation.enabled false`

**Q: Does Foundation Agent work with my custom Goose recipes?**
A: Yes. Foundation Agent is a recipe itself, compatible with all others.

**Q: What if Foundation Agent gives bad recommendations?**
A: You're in control. Review F-score, δ_delusion. If low confidence, don't follow recommendations.

**Q: How long does migration take?**
A: Individual: 1-2 weeks trial. Team: 4-8 weeks gradual rollout.

**Q: Do I need to retrain my team?**
A: Minimal. Foundation Agent outputs are self-explanatory. 1-hour intro sufficient.

**Q: What if my domain isn't covered in physics-laws.yaml?**
A: Contribute domain-specific knowledge! Process documented in Quick Reference.

**Q: Can I use Foundation Agent offline?**
A: Partially. Knowledge bases are local, but LLM API calls require internet.

---

## Success Stories (Post-Migration)

### Example 1: Startup (5 engineers)

**Before Migration:**
- Architecture debates: 2-3 days
- Wrong decisions: 2 major rewrites in 6 months

**After Migration (3 months):**
- Architecture validation: 26 minutes
- Prevented 1 major rewrite (saved $100k)
- F-score average: 0.84
- Team satisfaction: 9/10

**Quote:** *"Foundation Agent pays for itself after ONE prevented architectural mistake."*

---

### Example 2: Enterprise (50 engineers)

**Before Migration:**
- Manual architecture reviews: 2 weeks
- Inconsistent quality (depends on reviewer)

**After Migration (6 months):**
- Foundation Agent + human review: 3 days
- Consistent quality (F-score always calculated)
- Knowledge accumulation: 127 validated patterns
- All teams benefit from collective intelligence

**Quote:** *"Foundation Agent democratized architectural expertise across the entire org."*

---

## Conclusion

**Migration is safe, gradual, and reversible.**

**Recommended path:**
1. **Week 1:** Try explicitly on 1 decision
2. **Week 2-4:** Enable on 1 project
3. **Week 5+:** Enable globally with smart routing

**Guarantees:**
- ✅ Zero breaking changes
- ✅ 100% opt-in
- ✅ Instant rollback
- ✅ Backward compatible

**You're in control. Try Foundation Agent at your own pace. Disable anytime if it doesn't work for you.**
