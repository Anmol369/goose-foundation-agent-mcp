# Foundation Agent Testing Strategy

**How to Validate That Foundation Agent Actually Works**

**Purpose:** Define comprehensive testing approach for Foundation Agent
**Audience:** Engineering team, QA, Contributors
**Status:** Ready for implementation

---

## Testing Philosophy

**Core Principle:** Foundation Agent makes predictions about reality. Testing validates predictions against actual measurements.

```
Foundation Agent says: "This is PHYSICS with F-score 0.86"
Testing verifies: Did reality match? Was F-score accurate?
```

---

## Test Pyramid

```
         ╱╲
        ╱  ╲     E2E Tests (5%)
       ╱────╲    • Full 4-phase workflow
      ╱      ╲   • Real systems
     ╱────────╲  • Validation against ground truth
    ╱          ╲
   ╱────────────╲
  ╱  Integration  ╲  Integration Tests (25%)
 ╱────────────────╲ • Phase-to-phase handoff
╱                  ╲• Quality gate enforcement
────────────────────
                     Unit Tests (70%)
                     • Individual tool functions
                     • Knowledge base queries
                     • Metric calculations
```

---

## 1. Unit Tests (70% of test suite)

### 1.1 Perception Agent - 7-Tool Framework

```rust
// crates/goose/src/foundation/agents/perception/tests.rs

#[cfg(test)]
mod perception_tests {
    use super::*;

    #[test]
    fn test_first_principles_decomposition() {
        let constraint = "Microservices architecture required";
        let result = first_principles_decomposition(constraint);

        assert_eq!(result.layers, 4);
        assert_eq!(result.physics_core, None); // No physics bedrock
        assert_eq!(result.assessment, ConstraintType::Construct);
    }

    #[test]
    fn test_human_existence_test() {
        // Physics constraint
        let physics = "Network latency due to distance";
        assert_eq!(
            human_existence_test(physics),
            (true, "Speed of light exists without humans")
        );

        // Construct
        let construct = "Must use REST API";
        assert_eq!(
            human_existence_test(construct),
            (false, "Human convention")
        );
    }

    #[test]
    fn test_unlimited_resources_test() {
        // Economic (removable with resources)
        let economic = "Can't afford servers";
        assert_eq!(unlimited_resources_test(economic), (true, "Economic limitation"));

        // Physics (not removable)
        let physics = "Speed of light limits latency";
        assert_eq!(unlimited_resources_test(physics), (false, "Physics limitation"));
    }

    #[test]
    fn test_idiot_index_calculation() {
        let current_cost = 100_000.0;
        let theoretical_min = 1_000.0;
        let index = calculate_idiot_index(current_cost, theoretical_min);

        assert_eq!(index, 100.0);
        assert_eq!(interpret_idiot_index(index), "MASSIVE artificial overhead");
    }

    #[test]
    fn test_constraint_classification_with_all_tools() {
        let constraint = ConstraintInput {
            description: "Microservices architecture",
            context: "Web application with 10k users",
        };

        let result = classify_constraint(&constraint, &knowledge_base);

        // All 7 tools should agree
        assert_eq!(result.tool_results.consensus_count(), 7);
        assert_eq!(result.classification_type, ConstraintType::Conventional);
        assert!(result.confidence > 0.90);
        assert!(result.delta_contribution > 0.75);
    }

    #[test]
    fn test_delta_delusion_calculation() {
        let constraints = vec![
            Constraint { delta_contribution: 0.80, weight: 0.3 },
            Constraint { delta_contribution: 0.15, weight: 0.5 },
            Constraint { delta_contribution: 0.40, weight: 0.2 },
        ];

        let delta = calculate_delta_delusion(&constraints);

        // Weighted average: 0.80*0.3 + 0.15*0.5 + 0.40*0.2 = 0.395
        assert!((delta - 0.395).abs() < 0.01);
    }
}
```

### 1.2 Architecture Agent - Q-D-S-A-A Algorithm

```rust
#[cfg(test)]
mod architecture_tests {
    use super::*;

    #[test]
    fn test_question_phase() {
        let requirement = "Need microservices for scalability";
        let result = question_requirement(&requirement);

        assert!(result.questions.len() >= 3); // At least 3 "why?" levels
        assert!(result.justified == false); // Not justified without measurement
    }

    #[test]
    fn test_delete_phase() {
        let components = vec![
            Component { name: "Service A", justification: "No clear reason" },
            Component { name: "Service B", justification: "Proven bottleneck" },
        ];

        let result = apply_deletion(&components);

        assert_eq!(result.deleted.len(), 1); // Delete Service A
        assert_eq!(result.retained.len(), 1); // Keep Service B
    }

    #[test]
    fn test_complexity_calculation() {
        let baseline = Architecture {
            components: 12,
            interfaces: 66, // n(n-1)/2 = 12*11/2 = 66
        };

        let proposed = Architecture {
            components: 4,
            interfaces: 6, // 4*3/2 = 6
        };

        let metrics = calculate_complexity_metrics(&baseline, &proposed);

        assert_eq!(metrics.component_reduction, 0.67); // (12-4)/12 = 67%
        assert_eq!(metrics.interface_reduction, 0.91); // (66-6)/66 = 91%
    }

    #[test]
    fn test_pattern_alignment() {
        let patterns = vec![
            Pattern { name: "Flow Dynamics", is_natural: true },
            Pattern { name: "Minimal Encoding", is_natural: true },
            Pattern { name: "Custom Pattern", is_natural: false },
        ];

        let alignment = calculate_pattern_alignment(&patterns);

        assert_eq!(alignment, 0.67); // 2 of 3 are natural
    }
}
```

### 1.3 Execution Agent - Bottleneck Detection

```rust
#[cfg(test)]
mod execution_tests {
    use super::*;

    #[test]
    fn test_singular_constraint_identification() {
        let bottlenecks = vec![
            Bottleneck { location: "Database", impact: 800 },
            Bottleneck { location: "Network", impact: 150 },
            Bottleneck { location: "CPU", impact: 50 },
        ];

        let singular = identify_singular_constraint(&bottlenecks);

        assert_eq!(singular.location, "Database");
        assert_eq!(singular.dominance_ratio, 800.0 / 150.0); // 5.3×
        assert!(singular.is_singular); // Ratio > 1.5
    }

    #[test]
    fn test_critical_path_analysis() {
        let graph = DependencyGraph {
            nodes: vec![
                Node { id: "A", duration: 5, dependencies: vec![] },
                Node { id: "B", duration: 10, dependencies: vec!["A"] },
                Node { id: "C", duration: 8, dependencies: vec!["B"] },
                Node { id: "D", duration: 15, dependencies: vec!["B"] },
            ],
        };

        let critical_path = analyze_critical_path(&graph);

        // A → B → D (5 + 10 + 15 = 30)
        assert_eq!(critical_path.path, vec!["A", "B", "D"]);
        assert_eq!(critical_path.duration, 30);
    }

    #[test]
    fn test_resource_concentration_multiplier() {
        let concentrated = 0.8;
        let distributed = 0.2;
        let gamma = 1.7;

        let multiplier = calculate_velocity_multiplier(concentrated, distributed, gamma);

        // (0.8 / 0.2)^1.7 = 4.0^1.7 ≈ 9.2
        assert!((multiplier - 9.2).abs() < 0.5);
    }

    #[test]
    fn test_e_flow_calculation() {
        let total_time = 1000.0;
        let productive_time = 810.0;

        let e_flow = calculate_e_flow(productive_time, total_time);

        assert_eq!(e_flow, 0.81);
    }
}
```

### 1.4 Adaptation Agent - Learning Metrics

```rust
#[cfg(test)]
mod adaptation_tests {
    use super::*;

    #[test]
    fn test_f_score_calculation() {
        let r_validated = 10;
        let r_total = 12;
        let delta_delusion = 0.18;

        let f_score = calculate_f_score(r_validated, r_total, delta_delusion);

        // (10/12) × (1 - 0.18) = 0.833 × 0.82 = 0.683
        assert!((f_score - 0.683).abs() < 0.01);
    }

    #[test]
    fn test_learning_velocity() {
        let f_current = 0.83;
        let f_previous = 0.76;
        let cycles = 10;

        let df_dt = calculate_learning_velocity(f_current, f_previous, cycles);

        // (0.83 - 0.76) / 10 = 0.007
        assert!((df_dt - 0.007).abs() < 0.001);
    }

    #[test]
    fn test_learning_rate() {
        let df_dt = 0.009;
        let e_change = 0.005;

        let lr = calculate_learning_rate(df_dt, e_change);

        // 0.009 / 0.005 = 1.8
        assert_eq!(lr, 1.8);
        assert_eq!(interpret_lr(lr), LearningStatus::Thriving);
    }
}
```

### 1.5 Knowledge Base Queries

```rust
#[cfg(test)]
mod knowledge_base_tests {
    use super::*;

    #[test]
    fn test_query_physics_constraint() {
        let kb = KnowledgeBase::load().unwrap();
        let result = kb.query_physics("network_physics.speed_of_light");

        assert!(result.is_some());
        assert_eq!(result.unwrap().value, "299,792,458 m/s");
    }

    #[test]
    fn test_query_human_construct() {
        let kb = KnowledgeBase::load().unwrap();
        let result = kb.query_construct("architectural_patterns.microservices");

        assert!(result.is_some());
        assert_eq!(result.unwrap().type_field, "CONSTRUCT");
    }

    #[test]
    fn test_pattern_lookup() {
        let kb = KnowledgeBase::load().unwrap();
        let pattern = kb.find_validated_pattern("database-n+1-001");

        assert!(pattern.is_some());
        assert!(pattern.unwrap().success_rate > 0.85);
    }
}
```

---

## 2. Integration Tests (25% of test suite)

### 2.1 Phase-to-Phase Handoff

```rust
#[tokio::test]
async fn test_perception_to_architecture_handoff() {
    let system_description = "Microservices with 500ms latency";

    // Phase 1: Perception
    let perception_output = run_perception_agent(system_description).await.unwrap();

    // Verify Phase 1 output structure
    assert!(perception_output.summary.total_constraints > 0);
    assert!(perception_output.metrics.delta_delusion < 0.25);

    // Phase 2: Architecture (consumes Phase 1)
    let architecture_output = run_architecture_agent(&perception_output).await.unwrap();

    // Verify Architecture used Perception's classifications
    assert_eq!(
        architecture_output.construct_challenges.len(),
        perception_output.summary.conventional_count
    );
}

#[tokio::test]
async fn test_architecture_to_execution_handoff() {
    let perception_output = load_test_perception_output();
    let architecture_output = run_architecture_agent(&perception_output).await.unwrap();

    // Phase 3: Execution (consumes Phase 1 + 2)
    let execution_output = run_execution_agent(
        &perception_output,
        &architecture_output
    ).await.unwrap();

    // Verify Execution validated Architecture's blueprint
    assert!(execution_output.validation_against_agent2.architecture_validated);
}
```

### 2.2 Quality Gate Enforcement

```rust
#[tokio::test]
async fn test_quality_gate_1_enforcement() {
    let bad_perception = PerceptionOutput {
        metrics: Metrics {
            delta_delusion: 0.35, // Exceeds 0.25 threshold
            avg_confidence: 0.70, // Below 0.80 threshold
        },
        ..Default::default()
    };

    let result = validate_quality_gate_1(&bad_perception);

    assert!(result.is_err());
    assert!(result.unwrap_err().to_string().contains("delta_delusion exceeds threshold"));
}

#[tokio::test]
async fn test_quality_gate_2_enforcement() {
    let bad_architecture = ArchitectureOutput {
        complexity_metrics: ComplexityMetrics {
            reduction_percentage: 0.25, // Below 0.40 threshold
        },
        ..Default::default()
    };

    let result = validate_quality_gate_2(&bad_architecture);

    assert!(result.is_err());
    assert!(result.unwrap_err().to_string().contains("Complexity reduction below 40%"));
}
```

### 2.3 Knowledge Base Persistence

```rust
#[tokio::test]
async fn test_pattern_encoding_persistence() {
    let new_pattern = ValidatedPattern {
        pattern_id: "test-pattern-001".to_string(),
        validation_count: 1,
        success_rate: 0.92,
        ..Default::default()
    };

    // Encode pattern
    let kb = KnowledgeBase::load().unwrap();
    kb.encode_pattern(&new_pattern).await.unwrap();

    // Reload knowledge base
    let kb_reloaded = KnowledgeBase::load().unwrap();
    let retrieved = kb_reloaded.find_validated_pattern("test-pattern-001");

    assert!(retrieved.is_some());
    assert_eq!(retrieved.unwrap().success_rate, 0.92);

    // Cleanup
    kb.remove_pattern("test-pattern-001").await.unwrap();
}
```

---

## 3. End-to-End Tests (5% of test suite)

### 3.1 Full 4-Phase Workflow

```rust
#[tokio::test]
async fn test_complete_foundation_agent_workflow() {
    let system_description = r#"
        E-commerce API with microservices architecture.
        8 services, average response time 500ms, target <100ms.
        Database queries seem slow (~45ms).
        Team wants to switch to GraphQL.
    "#;

    // Run full Foundation Agent
    let result = run_foundation_agent(system_description.to_string()).await.unwrap();

    // Verify all phases completed
    assert!(result.quality_gates.phase_1_passed);
    assert!(result.quality_gates.phase_2_passed);
    assert!(result.quality_gates.phase_3_passed);
    assert!(result.quality_gates.phase_4_passed);

    // Verify final metrics
    assert!(result.foundation_metrics.f_score > 0.8);
    assert!(result.foundation_metrics.delta_delusion < 0.2);

    // Verify handoff package ready
    assert!(result.process_agent_handoff.ready_for_handoff);

    // Verify specific predictions
    assert!(result.executive_summary.key_findings
        .iter()
        .any(|f| f.contains("Microservices")));
    assert!(result.executive_summary.key_findings
        .iter()
        .any(|f| f.contains("Database")));
}
```

### 3.2 Validation Against Ground Truth

```rust
#[tokio::test]
async fn test_validation_against_ground_truth_dataset() {
    // Load 50 manually-reviewed systems
    let ground_truth = load_ground_truth_dataset();

    let mut correct_classifications = 0;
    let mut total_classifications = 0;

    for system in ground_truth {
        let result = run_foundation_agent(system.description).await.unwrap();

        for (idx, constraint) in result.phase_1_perception.constraints.iter().enumerate() {
            let expected = &system.expert_classifications[idx];

            total_classifications += 1;
            if constraint.classification.type_field == expected.type_field {
                correct_classifications += 1;
            }
        }
    }

    let accuracy = correct_classifications as f64 / total_classifications as f64;

    // Target: >85% accuracy vs expert human classification
    assert!(accuracy > 0.85, "Accuracy: {}", accuracy);
}
```

---

## 4. Ground Truth Dataset

### 4.1 Dataset Structure

```yaml
# tests/fixtures/ground-truth/system-001.yaml
system_id: "ecommerce-api-001"
description: |
  E-commerce REST API with microservices.
  8 services, 500ms avg response time.
  PostgreSQL database, Redis cache.
  50k users, 10k products.

expert_classifications:
  - constraint: "Microservices architecture required"
    type: CONVENTIONAL
    confidence: 0.95
    rationale: "No measured independent scaling needs"
    expert: "John Doe, Senior Architect, 15 years experience"

  - constraint: "Database queries slow (45ms)"
    type: PHYSICS
    sub_type: WITH_CONSTRUCT_OVERLAY
    confidence: 0.80
    rationale: "Disk I/O is physics, but 45ms suggests N+1 or missing indexes"
    expert: "Jane Smith, Database Expert, 10 years experience"

actual_implementation_results:
  - action: "Consolidated microservices to monolith"
    impact: "7× performance improvement"
    validates: "Microservices was CONSTRUCT classification"

  - action: "Added database indexes + eager loading"
    impact: "Database time: 45ms → 6ms"
    validates: "CONSTRUCT overlay (N+1 queries) correct"

metadata:
  system_type: "web-app"
  domain: "ecommerce"
  scale: "medium"
  date_analyzed: "2025-01-15"
  reviewed_by: 3
```

### 4.2 Ground Truth Collection Process

```markdown
## How to Create Ground Truth Entries

1. **Select Real System**
   - Open-source project OR
   - Anonymized production system

2. **Expert Manual Review** (3+ experts)
   - Senior architects classify each constraint
   - Apply 7-tool framework manually
   - Document rationale
   - Calculate confidence

3. **Implementation Validation**
   - Follow expert recommendations
   - Measure actual impact
   - Validate classifications

4. **Add to Dataset**
   - Minimum 50 systems for statistical significance
   - Diverse: web apps, mobile, distributed, embedded
   - Various scales: small, medium, large

5. **Continuous Update**
   - Add new systems quarterly
   - Re-validate existing systems annually
```

---

## 5. Regression Testing

### 5.1 Regression Test Suite

```rust
#[tokio::test]
async fn test_regression_known_good_architectures() {
    let known_good = vec![
        ("shopify-monolith", 0.89),
        ("netflix-microservices", 0.83),
        ("basecamp-monolith", 0.91),
    ];

    for (system, expected_f_score) in known_good {
        let description = load_system_description(system);
        let result = run_foundation_agent(description).await.unwrap();

        // F-score should remain stable (within 5%)
        let diff = (result.foundation_metrics.f_score - expected_f_score).abs();
        assert!(
            diff < 0.05,
            "F-score regression for {}: expected {}, got {}",
            system,
            expected_f_score,
            result.foundation_metrics.f_score
        );
    }
}
```

---

## 6. Performance Testing

### 6.1 Timing Benchmarks

```rust
#[tokio::test]
async fn test_phase_duration_benchmarks() {
    let system_description = load_test_system();

    let start = Instant::now();
    let perception = run_perception_agent(&system_description).await.unwrap();
    let phase_1_duration = start.elapsed();

    assert!(phase_1_duration.as_secs() < 600); // <10 minutes

    let start = Instant::now();
    let architecture = run_architecture_agent(&perception).await.unwrap();
    let phase_2_duration = start.elapsed();

    assert!(phase_2_duration.as_secs() < 900); // <15 minutes

    // Total should be <30 minutes
    let total = phase_1_duration + phase_2_duration + /* ... */;
    assert!(total.as_secs() < 1800);
}
```

---

## 7. Error Handling Tests

### 7.1 Edge Cases

```rust
#[tokio::test]
async fn test_novel_domain_handling() {
    let description = "Quantum computing architecture for error correction";

    let result = run_perception_agent(description).await.unwrap();

    // Should mark as low confidence
    assert!(result.metrics.avg_confidence < 0.60);

    // Should recommend domain expert review
    assert!(result.validation_needed.iter()
        .any(|v| v.contains("novel domain")));
}

#[tokio::test]
async fn test_conflicting_tool_results() {
    let constraint = "Constraint with ambiguous classification";

    let result = classify_constraint_with_conflict(constraint);

    // Should be marked HYBRID
    assert_eq!(result.classification_type, ConstraintType::Hybrid);

    // Confidence should be reduced
    assert!(result.confidence < 0.70);
}
```

---

## 8. Test Coverage Requirements

```
Component                    Minimum Coverage
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Perception Agent (7 tools)   95%
Architecture Agent (Q-D-S-A) 90%
Execution Agent              90%
Adaptation Agent             85%
Knowledge Base Queries       100%
Orchestrator                 95%
Quality Gates                100%
Metrics Calculations         100%

Overall Target: >90% code coverage
```

---

## 9. Continuous Integration

```yaml
# .github/workflows/foundation-agent-tests.yml
name: Foundation Agent Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Run Unit Tests
        run: cargo test --package goose-foundation --lib

      - name: Run Integration Tests
        run: cargo test --package goose-foundation --test '*'

      - name: Run E2E Tests
        run: cargo test --package goose-foundation --test e2e

      - name: Check Coverage
        run: |
          cargo tarpaulin --out Xml
          bash <(curl -s https://codecov.io/bash)

      - name: Validate Against Ground Truth
        run: cargo test --test ground_truth_validation

      - name: Benchmark Performance
        run: cargo bench
```

---

## 10. Testing Checklist (Before Release)

**Pre-Release Validation:**

- [ ] All unit tests pass (>90% coverage)
- [ ] All integration tests pass
- [ ] E2E tests pass on 10+ real systems
- [ ] Ground truth validation >85% accuracy
- [ ] Regression tests show <5% variation
- [ ] Performance benchmarks <30 min total
- [ ] Edge cases handled gracefully
- [ ] Knowledge base queries validated
- [ ] Quality gates enforce thresholds correctly
- [ ] Pattern encoding persists correctly

**Ground Truth Dataset:**

- [ ] Minimum 50 systems analyzed
- [ ] 3+ expert reviews per system
- [ ] Diverse system types (web, mobile, distributed)
- [ ] Implementation results validate predictions
- [ ] Documented in tests/fixtures/ground-truth/

**Performance:**

- [ ] Phase 1: <10 minutes
- [ ] Phase 2: <15 minutes
- [ ] Phase 3: <12 minutes
- [ ] Phase 4: <5 minutes
- [ ] Total: <30 minutes (target: <26 minutes)

---

## Conclusion

**Testing Foundation Agent requires:**

1. **Unit tests** validate individual tools/functions
2. **Integration tests** validate phase handoffs and quality gates
3. **E2E tests** validate full workflow on real systems
4. **Ground truth dataset** validates accuracy vs expert human classification
5. **Regression tests** ensure stable predictions over time
6. **Performance tests** ensure acceptable duration (<30 min)
7. **Edge case tests** ensure graceful handling of uncertainty

**Target metrics:**
- >90% code coverage
- >85% accuracy vs expert classification
- <5% regression variance
- <30 minutes total duration
- 100% quality gate enforcement

**With comprehensive testing, Foundation Agent predictions are trustworthy and reproducible.**
