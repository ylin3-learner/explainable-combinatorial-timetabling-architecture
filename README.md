# Explainable Combinatorial Timetabling & Automated Conflict Refinement Engine

© 2026. All rights reserved.

> **Notice:** The production source code, specific constraint heuristics, and underlying database schemas of this optimization system are hosted in a secure, private repository due to proprietary intellectual property protections and ongoing commercial deployment. This public repository functions exclusively as a high-level architectural specification, theoretical formalization, and methodology showcase for graduate school application portfolios. Unauthorized duplication, modification, or commercial exploitation of these designs is strictly prohibited.

---

## 1. Architectural Philosophy & The Core Challenge
Industrial-grade constraint solvers (such as Google OR-Tools CP-SAT) are highly efficient at navigating dense search spaces. However, when real-world requirements become over-constrained, the solver outputs a binary, uninformative `INFEASIBLE` state. In practical scenarios—such as K-12 institutional scheduling involving complex overlapping human and physical resource constraints—this "black box" failure mode leaves operators without actionable diagnostic insights.

This project implements an **Explainable Constraint Programming (XCP)** architecture. Instead of treating explanation as a superficial post-hoc layer, this system embeds diagnostic logic into the compilation and state-evaluation pipelines. It automatically isolates root-cause contradictions, extracts **Minimal Unsatisfiable Subsets (MUS)**, and provides human-readable feedback to enable defensible relaxation choices.

---

## 2. High-Level Modular Topology

The system decoupled the data ingestion, configuration parsing, CP-SAT model compilation, and the explainable diagnostic pipeline into distinct boundaries to handle NP-hard computational scale cleanly.

```mermaid
graph TB
    classDef entry_node fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px,color:#0d47a1;
    classDef ui_node fill:#f1f8e9,stroke:#558b2f,stroke-width:2px,color:#2e7d32;
    classDef core_node fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef crypto_node fill:#ffebee,stroke:#d32f2f,stroke-width:2px,color:#b71c1c;
    classDef data_node fill:#eceff1,stroke:#607d8b,stroke-width:2px,color:#263238;
    classDef pipeline_node fill:#fff9c4,stroke:#fbc02d,stroke-width:2px,color:#f57f17;

    subgraph Entry["🚪 System Ingestion Entry"]
        MAIN["runtime_orchestrator.py<br/>Application Entry"]:::entry_node
        LAUNCHER["ui_bootstrapper.py<br/>Asynchronous UI Bootstrapper"]:::entry_node
    end

    subgraph GUI["🖥️ Human-in-the-Loop Control Console"]
        RUNNER["optimization_dashboard.py<br/>Solver Real-Time Console"]:::ui_node
        RESPONSIVE["viewport_adapter.py<br/>Dynamic Layout Engine"]:::ui_node
        EDITOR["constraint_manifest_editor/<br/>Rule Configuration Interface"]:::ui_node
        
        subgraph TABS["📋 Constraint Config Panels"]
            TAB_DASH["diagnostic_telemetry<br/>Pre-solve Diagnostic Console"]
            TAB_SWAP["dynamic_mutation<br/>Manual Overrides & Swapping"]
            TAB_SYNC["temporal_alignment<br/>Slot Synchronization Panel"]
            TAB_RESTRICT["forbidden_spans<br/>Restricted Spans Layout"]
            TAB_CONCUR["capacity_bounds<br/>Concurrency Boundary Config"]
            TAB_LOCKED["anchored_slots<br/>Locked Temporal Slots"]
            TAB_STRICT["resource_compaction<br/>Curriculum Density Recommender"]
            TAB_PREF["spatial_preference<br/>Location Affinity Matrix"]
            TAB_STAB["schedule_stability<br/>Stability Preservation Constraints"]
            TAB_CONSEC["consecutive_blocks<br/>Block Scheduling Weights"]
            TAB_TEACHER["instructor_load<br/>Faculty Load Distribution Optimization"]
            TAB_ROTATE["interleaved_rotation<br/>Alternating Bi-weekly Rotation"]
        end
    end

    subgraph Scheduler["🧠 Explainable Constraint Programming Core"]
        CONST["system_manifest.py<br/>Global Envs & Asset Constants"]:::core_node
        LOADER["data_ingestion_layer.py<br/>Unified Schedule Datastore"]:::core_node
        RULES["primitive_constraints.py<br/>Atomic Constraint Operators (8 Primitives)"]:::core_node
        CFIX["invariant_rules.py<br/>Structural Hard Constraints (14 Invariants)"]:::core_node
        VAR["variant_rules.py<br/>User-Defined Variant Rules"]:::core_node
        ROT["rotation_calculus.py<br/>Bi-weekly Interleaved Operations"]:::core_node
        PREF["soft_objectives.py<br/>Heuristic Preference Weights"]:::core_node
        BUILD["model_compiler.py<br/>CP-SAT Model Compiler & Solver Engine"]:::core_node
        DIAG["conflict_diagnostics.py<br/>Pre/Post-Infeasibility Diagnostic Engine"]:::core_node
        TGRAPH["bipartite_instructor_graph.py<br/>Bipartite Topology & Graph Relational Mapper"]:::core_node
        SWAP["mutation_engine.py<br/>Stateful Conflict-Free Swapping Engine"]:::core_node
        WEEK["temporal_calendar.py<br/>Temporal Matrix Calculator"]:::core_node
        EXPORT["serialization_layer.py<br/>Matrix Result Exporter"]:::core_node
    end

    subgraph Application["🔗 Architectural Bridge Layer"]
        STARTUP["lifecycle_manager.py<br/>Lifecycle State Machine"]:::crypto_node
        PERM["security_context.py<br/>RBAC & Scope Evaluator"]:::crypto_node
    end

    subgraph Licensing["🔐 Cryptographic Attestation Subsystem"]
        LMODEL["attestation_schema.py<br/>Cryptographic Identity Struct"]:::crypto_node
        VERIFY["ed25519_sign_validator.py<br/>Digital Signature Verification"]:::crypto_node
        MACHINE["hardware_fingerprint.py<br/>Hardware Fingerprint Synthesis"]:::crypto_node
        ACTIVATE["node_binding_orchestrator.py<br/>Hardware-Node Cryptographic Binding"]:::crypto_node
        AUDIT["tamper_proof_audit.py<br/>Cryptographic Auditing Records"]:::crypto_node
    end

    subgraph Data["📊 Schema Storage & Asset Lake"]
        RAW["raw/source_manifest.xlsx"]:::data_node
        GEN["intermediate/<br/>Serialized Graph Model"]:::data_node
        RULESDIR["policy_rules/<br/>JSON Encrypted Policies"]:::data_node
    end

    subgraph Tools["🛠️ Continuous Data Extraction Pipeline"]
        PREPARE["pipeline_orchestrator.py<br/>ETL Ingestion Engine"]:::pipeline_node
        PARSER["schema_extractor.py<br/>Relational Data Extractor"]:::pipeline_node
        BITMASK["bitmask_tensor_generator.py<br/>Bitmask Vector Generation"]:::pipeline_node
        GENREQ["requirement_synthesizer.py<br/>Demand Matrix Synthesizer"]:::pipeline_node
    end

    MAIN --> LAUNCHER
    MAIN -.-> STARTUP
    LAUNCHER --> RUNNER
    LAUNCHER --> EDITOR
    EDITOR --> TABS
    
    STARTUP --> LMODEL
    STARTUP --> PERM
    LMODEL --> VERIFY
    LMANAGER --> MACHINE
    LMANAGER --> ACTIVATE
    LMANAGER --> AUDIT

    RUNNER --> BUILD
    RUNNER --> PERM
    BUILD --> LOADER
    BUILD --> CFIX
    BUILD --> VAR
    BUILD --> ROT
    BUILD --> PREF
    BUILD --> RULES
    BUILD --> DIAG
    BUILD --> EXPORT
    
    TABS --> RULESDIR
    EDITOR --> Data
    DIAG --> TGRAPH
    DIAG --> SWAP
    
    PREPARE --> PARSER
    PREPARE --> BITMASK
    PARSER --> RAW
    BITMASK --> RAW
    PREPARE --> GEN
    GEN --> LOADER
    RULESDIR --> VAR
    RULESDIR --> CFIX
    RULESDIR --> PREF

    EXPORT --> Data
```

---

## 3. End-to-End Optimization & Diagnostic Flow
Data-level inputs are vectorized into high-density tensors before entering the CP-SAT compiler. If structural invariants or dynamic policy overlays collide, the control flow is instantly routed into an isolation sequence.

```mermaid
flowchart TD
    %% ==========================================
    %% STYLE DEFINITIONS (Dataflow Friendly)
    %% ==========================================
    classDef input_style fill:#eceff1,stroke:#607d8b,stroke-width:2px,color:#263238;
    classDef pipeline_style fill:#fff9c4,stroke:#fbc02d,stroke-width:2px,color:#f57f17;
    classDef load_style fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px,color:#0d47a1;
    classDef compiler_style fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef solver_style fill:#ede7f6,stroke:#b39ddb,stroke-width:2px,color:#4a148c;
    classDef branch_style fill:#ffebee,stroke:#d32f2f,stroke-width:2px,color:#b71c1c;
    classDef diag_style fill:#fbe9e7,stroke:#ff5722,stroke-width:2px,stroke-dasharray: 3 3,color:#bf360c;

    XLSX["📊 source_manifest.xlsx<br/>Faculty Load / Demands / Allocation Bounds"]:::input_style
    
    subgraph PREP["🛠️ High-Throughput Ingestion Subsystem (ETL)"]
        P1["schema_extractor.py<br/>Extracts Resource Matrix"]:::pipeline_style
        P2["bitmask_tensor_generator.py<br/>Vectorizes Forbidden Availability Spans"]:::pipeline_style
        P3["pipeline_orchestrator.py<br/>Synchronizes ETL Subsystem"]:::pipeline_style
    end
    
    JSON1["serialized_instructor_demands.json<br/>Validated Allocations Matrix"]:::input_style
    JSON2["temporal_availability_bitmasks.json<br/>Vectorized Availability Constraints"]:::input_style
    
    subgraph LOAD["📥 Decoupled Data Loader Layer"]
        SD["ScheduleDatastore<br/>data_ingestion_layer.py"]:::load_style
        SD1["Structural Entity Bounds"]:::load_style
        SD2["Faculty Load Vectors"]:::load_style
        SD3["Availability Tensors"]:::load_style
        SD4["Dynamic Exception Masks"]:::load_style
    end
    
    subgraph MODEL["🏗️ CP-SAT Constrained Model Compiler"]
        VAR["Variable Initialization<br/>Decision Variable Matrix: x[class, subject, day, slot]"]:::compiler_style
        STRUCT["Structural Bound Consolidation<br/>- Strict Volumetric Allocation Check<br/>- Uniqueness Injection (Class Domain)<br/>- Conflict Prevention (Instructor Domain)"]:::compiler_style
        FIXED["invariant_rules.py<br/>14 Structural Invariant Core Rules"]:::compiler_style
        ROT["rotation_calculus.py<br/>Bi-weekly Alternating Transformations"]:::compiler_style
        VARIABLE["variant_rules.py<br/>Dynamic Overlays: Sync / Concurrency Limits"]:::compiler_style
        SOFT["soft_objectives.py<br/>Heuristic Objective Regularization Weights"]:::compiler_style
    end
    
    SOLVER["🔬 Industrial SAT/CP Solver Core<br/>Bounded Search Horizon: max_time=40s"]:::solver_style
    
    subgraph RESULT["📊 Operational Inversion Boundaries"]
        FEASIBLE["✅ Feasible / Globally Optimal Solution"]:::branch_style
        INFEASIBLE["❌ Infeasible Constraint Over-Constrained State"]:::branch_style
    end
    
    subgraph POST["📤 Materialization & Output Verification"]
        SNAP["solution_snapshot.json<br/>Immutable Matrix Snapshot"]:::load_style
        POSTDIAG["Post_Solve_Verification<br/>Idempotency & Load Audit Pass"]:::load_style
        EXP["serialization_layer.py<br/>Vector Matrix Visualizer Engine"]:::load_style
        WATERMARK["Attestation_Watermark_Overlay<br/>Appends Cryptographic Context Node"]:::crypto_node
    end
    
    subgraph DIAGFIX["🔬 Explainable Constraint Programming (XCP) Conflict Isolation"]
        DF1["Incremental Relaxation Search<br/>Isolates Cumulative Over-Constraining Sets"]:::diag_style
        DF2["Exclusionary Core Isolation<br/>Prunes Non-Critical Mutated Constraints"]:::diag_style
        DF3["Variant Boundary Diagnostic<br/>Pinpoints Overlapping Dynamic Policy Failures"]:::diag_style
        DF4["Bipartite Constraint Conflict Mapping<br/>Identifies Multi-Variable Rule Collisions"]:::diag_style
        DF5["Bipartite Graph Hall's Marriage Verifier<br/>Extracts Structural Bottleneck Cuts"]:::diag_style
    end
    
    XLSX --> P3
    P3 --> P1 --> JSON1
    P3 --> P2 --> JSON2
    JSON1 --> SD
    JSON2 --> SD
    SD --> SD1 & SD2 & SD3 & SD4
    
    SD --> VAR
    VAR --> STRUCT
    STRUCT --> FIXED
    FIXED --> ROT
    ROT --> VARIABLE
    VARIABLE --> SOFT
    SOFT --> SOLVER
    
    SOLVER --> FEASIBLE
    SOLVER --> INFEASIBLE
    
    FEASIBLE --> SNAP
    SNAP --> POSTDIAG
    POSTDIAG --> EXP
    EXP --> WATERMARK
    
    INFEASIBLE --> DF1
    DF1 --> DF2
    DF2 --> DF3
    DF3 --> DF4
    DF4 --> DF5
```

4. Deep-Dive: Advanced Infeasibility Diagnosis Pipeline

When the solver hits an unsat condition, the system launches a multi-tiered search architecture to systematically isolate the minimal conflicting core without causing search state explosions.
```mermaid
flowchart TD
    %% ==========================================
    %% STYLE DEFINITIONS (Patent / Paper Style)
    %% ==========================================
    classDef init_state fill:#ffebee,stroke:#d32f2f,stroke-width:2px,color:#b71c1c;
    classDef phase_0 fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px,color:#0d47a1;
    classDef phase_1 fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100;
    classDef phase_2 fill:#f1f8e9,stroke:#558b2f,stroke-width:2px,color:#2e7d32;
    classDef phase_3 fill:#ede7f6,stroke:#b39ddb,stroke-width:2px,color:#4a148c;
    classDef phase_4 fill:#fbe9e7,stroke:#ff5722,stroke-width:2px,color:#bf360c;
    classDef report_node fill:#e0f7fa,stroke:#00bcd4,stroke-width:2px,color:#006064;

    START["❌ Solver Returns INFEASIBLE<br/>(Over-Constrained State)"]:::init_state
    
    subgraph P0["🩺 Phase 0: Pre-Solve Structural Filtering"]
        PRE1["Volumetric Load Ratio Check<br/>(Total Demands vs. Total Temporal Space)"]
        PRE2["Bipartite Matching Density Check<br/>(Hall's Marriage Theorem Tight-Cut Extraction)"]
    end
    style P0 fill:#e3f2fd,stroke:#1e88e5,stroke-width:1px

    subgraph P1["🔬 Phase 1: Invariant Constraint Diagnostic (IRI)"]
        D1["Initialize Empty Base Constraint Set ∅<br/>Incrementally Inject Structural Invariants"]
        D1A{"Evaluate Solver State<br/>Post-Injection"}
        D1B["✅ Feasible → Maintain State<br/>Inject Next Sequence"]
        D1C["⚠️ Search Timeout → Mutate State<br/>Re-evaluate with Horizon Window (60s)"]
        D1D["❌ Infeasible → Log Contradictory Node<br/>Terminate & Break Sequence Loop"]
    end
    style P1 fill:#fff3e0,stroke:#f57c00,stroke-width:1px

    subgraph P2["🔬 Phase 2: Invariant Back-Substitution Profiling (ESI)"]
        D2A["Establish Global Failure Vector<br/>Verify Infeasibility of Full System"]
        D2B["Sequential Exclusion Loop<br/>Mask One Invariant Constraint Rule At a Time"]
        D2C["✅ Transitioned to Feasible State<br/>→ Isolate Rule as Minimal Conflict Element"]
        D2D["❌ Remains Infeasible State<br/>→ Advance to Next Invariant Subspace"]
    end
    style P2 fill:#f1f8e9,stroke:#558b2f,stroke-width:1px

    subgraph P3["🔬 Phase 3: Variant Dynamic Policy Diagnostic"]
        D3A["Isolate Base Core Invariants<br/>Verify Dynamic Policy Null State Validity"]
        D3B["Layered Overlay Transformations<br/>Co-inject Co-locational, Allocational,<br/>& Capacity Boundary Overlays"]
        D3C["Relational Graph Analytics Cross-Check<br/>Map Multi-Variable Entanglements"]
    end
    style P3 fill:#ede7f6,stroke:#b39ddb,stroke-width:1px

    subgraph P4["🔬 Phase 4: Pairwise Combinatorial Collision Analysis"]
        D4A["Generate Tuple Combinations<br/>Sort by Constraint Density Factor"]
        D4B["Bounded Horizon Execution<br/>Evaluate Top 15 High-Risk Conflict Vectors"]
        D4C["❌ Pairwise Solver Infeasibility<br/>→ Identify Joint Mutex Contradiction"]
    end
    style P4 fill:#fbe9e7,stroke:#ff5722,stroke-width:1px
    
    REPORT["📋 Root-Cause Telemetry Report<br/>• Isolated Minimal Conflict Sets (MCS)<br/>• Affected Graph Topologies (Resource / Class)<br/>• Defensible Architectural Relaxation Actions"]:::report_node
    
    START --> P0
    P0 --> P1
    D1 --> D1A
    D1A --> D1B
    D1A --> D1C
    D1A --> D1D
    D1C --> D1A
    
    P1 --> P2
    D2A --> D2B
    D2B --> D2C & D2D
    D2C --> REPORT
    
    P2 --> P3
    D3A --> D3B
    D3B --> D3C
    D3C --> REPORT
    
    P3 --> P4
    D4A --> D4B
    D4B --> D4C
    D4C --> REPORT
```

• Phase 0 (Pre-Solve Fast Pass): Extracts structural bottlenecks via graph density bounds (Hall's Condition cuts) before invoking the heavy solver loop.

• Phase 1 (Incremental Relaxation Injection): Progressively builds the hard constraint matrix to capture the earliest point of structural failure under a variable timeout window.

• Phase 2 (Exclusionary Subspace Isolation): Evaluates constraint deletion effects to mathematically isolate individual non-negotiable rule requirements.

• Phase 4 (Pairwise Combinatorial Collision): Ranks and tests high-density policy tuples to capture multi-variable mutex lockouts efficiently.
