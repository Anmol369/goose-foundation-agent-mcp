# Contributing to Foundation Agent

Thank you for your interest in contributing to Foundation Agent! This document provides guidelines for contributing validated patterns, physics constraints, and code improvements.

---

## 🎯 How You Can Contribute

### 1. Add Validated Patterns

**What are validated patterns?**
Real-world constraints you've encountered and validated through measurement or experimentation.

**How to contribute:**

```yaml
# recipes/foundation-agent/knowledge/validated-patterns.yaml

- pattern_id: "your-descriptive-pattern-id"
  constraint: "Clear description of the constraint"
  classification: "PHYSICS | CONSTRUCT | ECONOMIC | ORGANIZATIONAL"
  validation_method: "How you validated this (profiling, benchmarks, experiments)"
  confidence: 0.92  # 0.0-1.0 based on validation strength
  success_rate: 0.88  # How often this pattern holds true
  avg_improvement: "2.5× velocity"  # Quantified impact
  physics_basis: "Which physical law or principle this relates to"
  examples:
    - situation: "Specific scenario where this applied"
      before: "Metrics before applying pattern"
      after: "Metrics after applying pattern"
      improvement: "Quantified improvement"
  references:
    - "Link to paper, benchmark, or documentation"
```

**Example:**

```yaml
- pattern_id: "database-n-plus-one-queries"
  constraint: "N+1 queries cause linear performance degradation"
  classification: "PHYSICS"
  validation_method: "Profiled 50+ Rails applications, measured query counts vs response time"
  confidence: 0.95
  success_rate: 0.91
  avg_improvement: "8× faster response time"
  physics_basis: "Network latency (speed of light), database round-trip time"
  examples:
    - situation: "User listing page loading 100 users"
      before: "101 queries (1 + 100), 800ms response time"
      after: "2 queries (eager loading), 95ms response time"
      improvement: "8.4× faster"
  references:
    - "https://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations"
```

---

### 2. Add Physics Constraints

**What are physics constraints?**
Immutable limitations based on natural laws (not human opinions or conventions).

**How to contribute:**

```yaml
# recipes/foundation-agent/knowledge/physics-laws.yaml

category_name:  # e.g., network_physics, computation_physics
  your_constraint_name:
    value: "Precise quantitative measurement"
    source: "Research paper, RFC, or authoritative spec (with link)"
    practical_limit: "Real-world implications"
    implications:
      - "What this means for developers in practice"
      - "How this constraint affects system design"
    workarounds:
      - "How to work WITH this constraint (not against it)"
    measurement_method: "How to measure this in your system"
```

**Example:**

```yaml
network_physics:
  tcp_connection_overhead:
    value: "~40KB memory per connection (Linux kernel default)"
    source: "Linux kernel TCP stack documentation (https://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt)"
    practical_limit: "~10,000 concurrent connections per 1GB RAM"
    implications:
      - "C10K problem: 10,000 connections = 400MB memory minimum"
      - "Connection pooling essential for high-concurrency systems"
      - "Long-lived connections more efficient than connection-per-request"
    workarounds:
      - "Use connection pooling (reduces overhead to shared pool)"
      - "Implement HTTP/2 multiplexing (multiple requests per connection)"
      - "Tune kernel parameters (tcp_mem, tcp_rmem, tcp_wmem)"
    measurement_method: "ss -s | grep TCP (shows current TCP connections and memory)"
```

---

### 3. Report Issues or Suggest Improvements

**Found a bug?** [Open an issue](https://github.com/yourusername/foundation-agent/issues/new?template=bug_report.md)

**Have a feature request?** [Open a discussion](https://github.com/yourusername/foundation-agent/discussions/new?category=ideas)

**Documentation unclear?** [Suggest improvements](https://github.com/yourusername/foundation-agent/issues/new?template=documentation.md)

---

### 4. Improve Code or Documentation

**Code contributions:**
- Fix bugs in specialist agents
- Optimize performance (Phase 1 goal: 26 min → 18 min)
- Add domain-specific knowledge bases (mobile, embedded, ML)

**Documentation contributions:**
- Add examples to existing docs
- Improve clarity of explanations
- Translate documentation (future)

---

## 📋 Contribution Process

### Step 1: Fork & Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR-USERNAME/foundation-agent.git
cd foundation-agent
git checkout -b your-feature-branch
```

### Step 2: Make Changes

- Add your validated pattern or physics constraint
- Ensure proper YAML formatting
- Add examples and references
- Test locally (see Testing section below)

### Step 3: Commit & Push

```bash
git add .
git commit -m "Add validated pattern: your-pattern-id"
git push origin your-feature-branch
```

### Step 4: Open Pull Request

- Go to GitHub and open a Pull Request
- Describe what you're adding and why
- Include validation data or references
- Wait for review (typically 2-3 days)

---

## ✅ Testing Your Contributions

### Test Validated Patterns

```bash
# Run Foundation Agent with your new pattern
goose session start --recipe foundation-agent

# Describe a scenario where your pattern applies
> "I have a web app with N+1 query problems..."

# Verify Foundation Agent correctly identifies your pattern
```

### Test Physics Constraints

```bash
# Create a test scenario
goose session start --recipe foundation-agent/specialists/perception-agent

# Input a constraint that should match your physics law
> "My system needs to handle 100,000 TCP connections..."

# Verify correct classification and reference to your constraint
```

### Validate YAML Syntax

```bash
# Install yamllint
pip install yamllint

# Validate your changes
yamllint recipes/foundation-agent/knowledge/*.yaml
```

---

## 🎓 Quality Standards

### For Validated Patterns

- **Must have:** Real-world validation (not theoretical)
- **Must have:** Quantified metrics (before/after measurements)
- **Must have:** Confidence score based on sample size
- **Should have:** Multiple examples from different contexts
- **Should have:** References to supporting data

### For Physics Constraints

- **Must have:** Authoritative source (research paper, RFC, spec)
- **Must have:** Quantitative measurement (not "slow" but "100ms")
- **Must have:** Practical implications for developers
- **Should have:** Measurement method (how to verify in your system)
- **Should have:** Workarounds (how to work WITH the constraint)

### For Code Contributions

- **Must have:** Tests (unit, integration, or E2E as appropriate)
- **Must have:** Documentation (inline comments, README updates)
- **Should have:** Performance considerations (no regressions)
- **Should have:** Backward compatibility (unless major version)

---

## 📝 Style Guide

### YAML Style

```yaml
# Use 2-space indentation
# Use lowercase with hyphens for IDs
# Use sentence case for descriptions
# Include units in measurements

- pattern_id: "lowercase-with-hyphens"  # ✅ Good
  constraint: "Start with capital letter."  # ✅ Good
  value: "100ms (not just 100)"  # ✅ Good
  confidence: 0.92  # ✅ Good (0.0-1.0 range)

- pattern_id: "UPPERCASE_UNDERSCORES"  # ❌ Bad
  constraint: "all lowercase no period"  # ❌ Bad
  value: "100"  # ❌ Bad (missing units)
  confidence: 92  # ❌ Bad (use 0.0-1.0 range)
```

### Commit Messages

```bash
# Good commit messages
git commit -m "Add validated pattern: database-connection-pooling"
git commit -m "Fix: Perception agent classification confidence calculation"
git commit -m "Docs: Clarify Q-D-S-A-A algorithm examples"

# Bad commit messages
git commit -m "fixed stuff"  # ❌ Too vague
git commit -m "WIP"  # ❌ Don't commit WIP to main
git commit -m "Added a thing"  # ❌ Not descriptive
```

---

## 🏆 Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Credited in pattern/constraint metadata
- Invited to contributor discussions
- Eligible for "Top Contributor" badge (10+ merged PRs)

---

## ❓ Questions?

- **General questions:** [GitHub Discussions](https://github.com/yourusername/foundation-agent/discussions)
- **Contribution help:** [Discord/Slack community](#) (coming soon)
- **Security issues:** Email security@foundationagent.dev

---

## 📜 Code of Conduct

### Our Standards

- **Be respectful:** Disagree with ideas, not people
- **Be constructive:** Offer solutions, not just criticism
- **Be empirical:** Back claims with data and measurements
- **Be collaborative:** Help others learn and contribute

### Unacceptable Behavior

- Personal attacks or harassment
- Submitting false or misleading data
- Plagiarizing others' work
- Spamming or off-topic discussions

**Violations will result in warnings or bans at maintainers' discretion.**

---

## 🙏 Thank You!

Every contribution makes Foundation Agent more accurate and valuable for the entire engineering community. Whether you're adding a single validated pattern or improving documentation, you're helping teams avoid wasting resources on imaginary problems.

**"Measurement before assumption. Reality before convention. Physics before opinion."**

---

**Ready to contribute? [Open your first PR →](https://github.com/yourusername/foundation-agent/compare)**
