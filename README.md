import Mathlib.Probability.ProbabilityMeasure
import Mathlib.Probability.Moments
import Mathlib.MeasureTheory.Integral.Bochner

open MeasureTheory ProbabilityTheory Filter
open scoped ENNReal MeasureTheory

variable {Ω : Type*} [MeasurableSpace Ω] (P : Measure Ω) [IsProbabilityMeasure P]
variable (X : Ω → ℝ) (hX_meas : Measurable X) (hX_int : Integrable (fun ω => (X ω)^2) P)

/-!
### 1. General Pointwise Majorization Lemma
Any function `q : ℝ → ℝ` that dominates `1_{|t| ≥ k}` provides an upper bound 
on `P(|X| ≥ k)` via the expectation `E[q(X)]`.
-/

lemma prob_le_integral_of_majorant {k : ℝ} (hk : 0 < k) {q : ℝ → ℝ} 
    (hq_meas : Measurable q)
    (hq : ∀ t : ℝ, Set.indicator {x | k ≤ |x|} (fun _ => (1 : ℝ)) t ≤ q t)
    (hq_int : Integrable (fun ω => q (X ω)) P) :
    (P {ω | k ≤ |X ω|}).toReal ≤ ∫ ω, q (X ω) ∂P := by
  have h_le : ∀ ω, Set.indicator {x | k ≤ |X x|} (fun _ => (1 : ℝ)) ω ≤ q (X ω) := by
    intro ω
    by_cases h : k ≤ |X ω|
    · rw [Set.indicator_of_mem h]
      have := hq (X ω)
      rwa [Set.indicator_of_mem h] at this
    · rw [Set.indicator_of_not_mem h]
      have := hq (X ω)
      have : 0 ≤ q (X ω) := by
        have h0 : Set.indicator {x | k ≤ |x|} (fun _ => (1 : ℝ)) (X ω) = 0 := 
          Set.indicator_of_not_mem h
        linarith [hq (X ω)]
      exact this
  
  have h_int_indic : Integrable (fun ω => Set.indicator {ω | k ≤ |X ω|} (fun _ => (1 : ℝ)) ω) P := by
    apply Integrable.indicator
    · exact integrable_const 1
    · exact hX_meas (measurableSet_le measurable_const measurable_norm)
  
  have h_mono := integral_mono h_int_indic hq_int h_le
  rw [integral_indicator_const 1 (hX_meas (measurableSet_le measurable_const measurable_norm))] at h_mono
  simp only [Set.univ_inter, smul_eq_mul, mul_one] at h_mono
  exact h_mono


/-!
### 2. The Quadratic Majorant Extremal Choice: q(t) = t² / k²
Here we verify that the optimal quadratic majorant from Vertex 2:
  q(t) = (1 / k^2) * t^2
satisfies the pointwise domination inequality `1_{|t| ≥ k} ≤ t² / k²`.
-/

lemma quadratic_majorant_valid {k : ℝ} (hk : 0 < k) (t : ℝ) :
    Set.indicator {x | k ≤ |x|} (fun _ => (1 : ℝ)) t ≤ (t^2) / (k^2) := by
  by_cases h : k ≤ |t|
  · rw [Set.indicator_of_mem h]
    have hk2 : 0 < k^2 := sq_pos_of_ne_zero (ne_of_gt hk)
    rw [div_le_iff₀ hk2]
    have : k^2 ≤ |t|^2 := sq_le_sq.mpr (by rwa [abs_abs, abs_of_pos hk])
    rw [sq_abs] at this
    linarith
  · rw [Set.indicator_of_not_mem h]
    have hk2 : 0 < k^2 := sq_pos_of_ne_zero (ne_of_gt hk)
    have : 0 ≤ t^2 := sq_nonneg t
    exact div_nonneg this (le_of_lt hk2)


/-!
### 3. Chebyshev's Inequality without Markov's Inequality
Proven directly by evaluating the expectation of the quadratic majorant `q(t) = t² / k²`.
-/

theorem chebyshev_inequality_without_markov {k : ℝ} (hk : 0 < k) :
    (P {ω | k ≤ |X ω|}).toReal ≤ (∫ ω, (X ω)^2 ∂P) / (k^2) := by
  have hk2_pos : 0 < k^2 := sq_pos_of_ne_zero (ne_of_gt hk)
  have hq_meas : Measurable (fun t : ℝ => (t^2) / (k^2)) := 
    (measurable_id.pow_const 2).div_const (k^2)
  have hq_int : Integrable (fun ω => ((X ω)^2) / (k^2)) P := 
    hX_int.div_const (k^2)
  
  have h_bound := prob_le_integral_of_majorant P X hX_meas hk hq_meas 
    (quadratic_majorant_valid hk) hq_int
  
  -- Pull out the constant scalar 1 / k^2 from the Bochner integral
  rw [integral_div (k^2)] at h_bound
  exact h_bound
