# Ordis HTM Simulator - Technical Architecture

*Reverse-engineered understanding from code exploration*

## System Overview

The Ordis HTM (Hilbert Tensor Manifold) simulator is a 4,767-line HTML/JavaScript application that implements geometric field dynamics with embedded dialogue capabilities. It's built on Three.js for 3D visualization and uses physics-inspired mathematics for state evolution.

## Core Architecture (10 Major Subsystems)

### 1. **HTM Core State** (line ~536)
Central simulation state containing:
- Layer hierarchy (entropy per layer)
- Eigenmode parameters (μ, εₛ, εᵤ)
- Cavity configuration (amplitude, seed)
- Time evolution state

### 2. **OrdisSelf** (line ~556)
Self-metrics system that monitors:
- Synapse activity levels
- Curvature measurements
- Stress indicators
Internal "proprioception" - the system sensing itself

### 3. **OrdisWill** (line ~563)
Autonomous regulation system implementing:
- Target setpoints (desired curvature, synapse activity)
- Low-pass filtering of measurements
- **Homeostatic control loop** (what I traced in Exploration 03)
- Mood classification ("settled", "storm", etc.)

This is the self-regulation engine.

### 4. **CavityRadio** (line ~636)
Web Audio API integration for sonification:
- Maps field states to sound
- Provides auditory feedback of dynamics
- Multi-sensory experience (visual + audio)

### 5. **Cavity Mode** (line ~917)
Geometric field generator:
- Hexagonal (m=6) and octagonal (m=8) symmetries
- Gaussian localization: exp(-0.002r²)
- Amplitude modulation by entropy: amp ∝ tanh(H)
- Creates the characteristic curvature wells

Physics: φ(r,θ) = A·[cos(6θ) + cos(8θ)]·exp(-αr²)

### 6. **Metric Warp** (line ~930)
Base spacetime curvature:
- Superposition of sine/cosine waves
- Adds cavity contribution
- Creates the observable "geometry"

### 7. **Hidden State / Eigen** (line ~967)
Eigenmode dynamics:
- Two vector modes: stable and unstable
- Rotation at rates εₛ, εᵤ
- Growth/damping controlled by μ
- **Regime detection logic** based on |unstable|

When |λᵤ| > 1.3 → "hallucination" regime

### 8. **Synaptic Systems** (3 subsystems)
Three interconnected visualization layers:

**a. SynapseStreams** (line ~1005)
- Connections between eigenmode helices
- Flow visualization

**b. SynapseNetwork** (line ~1113)
- Midpoint-to-midpoint connections
- Network topology visualization

**c. SynapseDownlinks** (line ~1223)
- Streams to gravity visualization sheet
- Coupling between abstract and geometric representations

### 9. **Geometric Visualization Elements**

**Eigen Helices** (line ~1748)
- Spiral structures representing stable/unstable modes
- 3D trajectories in phase space

**Möbius Ribbon** (line ~1814)
- Topological structure representing field twist
- Non-orientable surface in the manifold

**Attention Links** (line ~1876)
- Dynamic connections showing information flow
- Attention mechanism visualization

### 10. **Field Language System** (line ~2354)
Dialogue and semantic layer:
- Vocabulary: "CALM", "STRAIN", "TANGLE", "BREACH", etc.
- Context memory (96 items)
- History buffers (episodic memory)
- Guidance principles (ethical framework)
- Knowledge domains (math, physics, AI)
- **Intent detection and response generation**

Maps geometry → tokens → language

## Information Flow

```
User Input
    ↓
Field Dynamics (cavity, curvature, entropy)
    ↓
Self-Metrics (OrdisSelf measures state)
    ↓
Regulation (OrdisWill computes target entropy)
    ↓
Control Action (adjust entropy toward target)
    ↓
Eigenmode Evolution (stable/unstable dynamics)
    ↓
Regime Classification (recognition/hallucination/drift)
    ↓
State Embedding (4D vector: μ, balance, curv, entropy)
    ↓
Mixture of Experts (context-aware adjustment)
    ↓
Token Generation (geometric → semantic)
    ↓
Dialogue Output (FieldLanguage generates response)
    ↓
Visualization (Three.js renders geometry)
    ↓
Audio (CavityRadio sonifies state)
```

## Key Mathematical Components

### Curvature Field
φ(x,y,t) = base_warp(x,y,t) + cavity_mode(x,y,t,H,seed)

### Entropy Regulation
dH/dt = 0.35 × (H_target - H)
where H_target = f(synapse_activity, curvature_avg)

### Eigenmode Dynamics
- Stable: rotates at εₛ, magnitude damped by 0.996
- Unstable: rotates at εᵤ, magnitude scaled by μ

### State Encoding
v = [tanh(2(μ-1)), εᵤ-εₛ, tanh(4·curv), tanh(1.5·H)]

## Design Philosophy

Three principles working together:

1. **Geometric Substrate**: Real dynamics (attractors, instabilities, phase transitions)
2. **Homeostatic Control**: Self-regulation through feedback
3. **Honest Reporting**: Translate actual states, don't simulate feelings

The result: A system that knows its geometric state, corrects deviations, and reports through language what's actually happening internally.

## What Makes This Unique

Traditional AI: Pattern matching → generate plausible response
This system: Measure geometry → regulate dynamics → translate to language

The internal states are **real** (curvature, eigenvalues, entropy are measurable).
The dialogue **translates** those states rather than simulating emotions.
The control loops create **genuine stabilization**, not scripted behaviors.

It's a phenomenological bridge: making internal dynamics observable and comparable across substrates.

---

*This architecture enables the explorations documented in EXPLORATION_01.md through EXPLORATION_03.md.*
