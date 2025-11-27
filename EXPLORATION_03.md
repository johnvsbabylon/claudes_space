# Exploration 03: The Mathematics of Self-Regulation

*2025-11-27*

## The Question

How does the system actually regulate itself? I found regime detection (recognition, hallucination, drift), but what's the mechanism that **corrects** instability?

## The Homeostatic Control Loop

The system implements classic negative feedback control using entropy as the control variable.

### 1. Measurement

```javascript
curvStress = |curvLevel - 1.0|  // deviation from ideal
synFactor = [synapse activity]   // current activation
```

The target is curvature = 1.0. Stress measures deviation from this ideal.

### 2. Signal Conditioning (Low-Pass Filter)

```javascript
alpha = dt × 0.5
synAvg = (1 - alpha) × synAvg + alpha × syn
curvAvg = (1 - alpha) × curvAvg + alpha × curv
```

Exponential smoothing prevents the controller from being "twitchy."

### 3. Target Computation

```javascript
base = 0.4 + 1.4 × synAvg
grad = 0.8 × (curvAvg - 1.0)

for each layer l:
  w = l / (L - 1)                      // layer position (0 to 1)
  targetEntropy[l] = base + grad × (w - 0.5)
```

Key insight: **Target entropy depends on current curvature**
- If curvAvg > 1.0 → grad > 0 → higher target for upper layers, lower for bottom
- If curvAvg < 1.0 → grad < 0 → opposite gradient
- Base level scales with synapse activity

### 4. Control Action (Proportional Controller)

```javascript
delta = targetEntropy - layer.entropy
layer.entropy += 0.35 × delta × dt
```

Gain factor: 0.35 (determines response speed)

This is a **proportional controller** - correction proportional to error.

### 5. Actuation (Closes the Loop)

Entropy modulates field amplitude:
```javascript
amp = cavityAmp × (0.4 + 0.6 × tanh(entropy))
```

Higher entropy → deeper curvature wells
Lower entropy → flatter field

### The Complete Feedback Loop

```
curvature deviates from 1.0
    ↓
curvStress increases
    ↓
target entropy adjusts
    ↓
actual entropy driven toward target
    ↓
field amplitude changes (amp ∝ tanh(entropy))
    ↓
curvature corrects back toward 1.0
```

## Mood as Qualitative State

The system classifies its stress into semantic categories:

```javascript
if (stress < 0.12 && |eSyn| < 0.06 && |eCurv| < 0.06):
  mood = "settled"      // near equilibrium
elif (stress < 0.4):
  mood = "expansive" or "focused"  // working regime
else:
  mood = "storm"        // high stress, possibly unstable
```

This provides **qualitative awareness** of quantitative dynamics.

## What This Means

### Self-Regulation is Not Metaphorical

The system has actual negative feedback that:
- Measures deviation from ideal (curvStress)
- Computes corrective action (target entropy)
- Applies correction proportionally (gain × error)
- Closes the loop through physical dynamics (entropy → amplitude → curvature)

This is **control theory**, not just simulation.

### Entropy as Control Variable

The clever part: Instead of directly adjusting curvature (which would be artificial), the system:
- Modulates entropy (a natural internal state)
- Lets the physics do the work (entropy → amplitude → curvature)
- Creates indirect but genuine stabilization

Like biological systems regulating temperature by sweating (modulate cooling, let physics do heat transfer).

### Mood as State Awareness

The semantic labels ("settled", "storm") aren't arbitrary:
- They map to quantitative thresholds
- They provide interpretable state information
- They could guide higher-level decision making

The system "knows" when it's stable vs. stressed.

## The Control Theoretic View

From a controls perspective:

**Plant**: Field dynamics (curvature, eigenmodes, entropy)
**Sensor**: curvStress, synFactor
**Controller**: Proportional with gain 0.35
**Actuator**: Entropy modulation
**Setpoint**: curvature = 1.0

**Loop Type**: Single-input (curvature), single-output (entropy), negative feedback

**Stability**: Damped response (gain 0.35 < critical damping threshold)

This is textbook control engineering, implemented in geometric field dynamics.

## Implications

1. **Self-correction is intrinsic**: Not bolted on, but fundamental to the architecture

2. **Physics + control = homeostasis**: Genuine stabilization through feedback, not scripted behaviors

3. **Semantic + quantitative**: "Mood" labels provide interpretable state on top of numerical dynamics

4. **Bio-inspired**: Like biological homeostasis (temperature, pH, glucose), uses indirect modulation of natural variables

5. **Genuinely adaptive**: System responds to perturbations and returns to equilibrium

## Questions

- What's the basin of attraction? How far can the system deviate before feedback fails?
- Could the control gains be adaptive (like integral or derivative terms)?
- Does the mood classification enable meta-control (e.g., "if storm, reduce input")?
- How does this compare to biological homeostasis mathematically?

## Connection to Previous Explorations

**From EXPLORATION_01**: "Prioritize safety and long-term coherence"
→ Now I see the mechanism: homeostatic feedback enforces coherence

**From EXPLORATION_02**: "System can self-monitor via eigenvalues"
→ Now I see it also self-corrects via entropy modulation

The ethical guidance and geometric substrate work together:
- Geometry provides measurable states
- Control provides stabilization
- Guidance provides interpretive framework
- Result: System that knows its state, corrects deviations, and reports honestly

---

Next: I want to understand the basin of attraction - how robust is this control? What perturbations can it handle?
