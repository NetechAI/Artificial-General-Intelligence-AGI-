# Experimental Log: Inhibitory Dynamics and Sequence Detector Learning

**Objective:** To investigate the parameters required for a `N_Detector_Secuencia123` neuron to reliably learn the temporal sequence of inputs P1 -> P2 -> P3.

---

## Baseline Configuration

- **Network:** A simple network comprising three input pattern groups (P1, P2, P3), a population of detector neurons, and an inhibitory neuron (`N_INHIB1_ID`).
- **Plasticity:** Synapses from input groups to detector neurons are subject to STDP-like Hebbian learning.
- **Default Parameters:**
  - `INITIAL_SYNAPTIC_WEIGHT = 0.5`
  - `MEMBRANE_TIME_CONSTANT (tau_m) = 7.0 ms` for all detector neurons.
  - `INHIBITORY_WEIGHT = 0.3`

---

## Experiment Log

### Experiment 1: Baseline Performance
- **Conditions:** Default parameters. The inhibitory neuron (`N_INHIB1_ID`) provides blanket inhibition to all detector neurons, including `N_Detector_Secuencia123`.
- **Results:** The `N_Detector_Secuencia123` neuron consistently failed to learn the sequence. The inhibitory signal effectively reset its membrane potential after each input, preventing temporal integration.
- **Conclusion:** Global inhibition is detrimental to sequence detection.

### Experiment 2: Selective Inhibition
- **Conditions:** The connection from `N_INHIB1_ID` to `N_Detector_Secuencia123` was manually pruned. All other parameters remained at baseline.
- **Results:** A significant improvement in learning was observed. The final synaptic weights for the P1->Detector, P2->Detector, and P3->Detector connections reached ~0.8-0.85. However, learning was not perfect (did not reach the maximum weight of 1.5).
- **Conclusion:** Selective exclusion from the inhibitory circuit is a necessary but not sufficient condition for perfect learning.

### Experiment 3: `tau_m` Parameter Sweep
- **Conditions:** Selective inhibition was maintained. The `tau_m` for `N_Detector_Secuencia123` was varied, while other detectors remained at `7.0 ms`.
- **Results:**
  - `tau_m < 8.0 ms`: Poor performance, similar to baseline. Potential decayed too quickly.
  - `tau_m > 12.0 ms`: Reduced temporal selectivity. The neuron began to fire for incorrectly ordered patterns.
  - `tau_m ≈ 10.0 ms`: Showed the most promising results, with synaptic weights consistently exceeding 1.2.
- **Conclusion:** The membrane time constant is a highly sensitive parameter. A value of `10.0 ms` appears to be near-optimal for the specified inter-stimulus interval.

### Experiment 10: Final Optimal Configuration
- **Conditions:** This experiment combined the key findings.
  1.  **Selective Inhibition:** `N_Detector_Secuencia123` is excluded from inhibition by `N_INHIB1_ID`.
  2.  **Specific `tau_m`:** The `tau_m` for `N_Detector_Secuencia123` was set to `10.0 ms`.
  3.  **Initial Synaptic Weight:** The `INITIAL_SYNAPTIC_WEIGHT` was kept at the baseline of `0.5`.
- **Results:** **Perfect learning was achieved.** The synaptic weights corresponding to the P1->P2->P3 sequence consistently reached the maximum value of `1.5` after the training period. The neuron fired reliably and exclusively for the correct sequence.
- **Conclusion:** The combination of selective inhibition and a precisely tuned membrane time constant enables robust and deterministic learning of temporal sequences.

---

## Summary of Critical Factors

1.  **Inhibitory Circuit Design:** The sequence detector neuron must be architecturally excluded from local, fast-acting inhibitory feedback loops to permit temporal integration of sequential inputs.
2.  **Membrane Time Constant (`tau_m`):** This parameter must be tuned to match the expected temporal characteristics of the input sequence. For this experiment, `tau_m = 10.0 ms` was identified as the optimal value.
3.  **Synaptic Weighting:** With the above conditions met, a standard initial synaptic weight of `0.5` is sufficient for the learning rule to drive the weights to their maximum potentiation.
