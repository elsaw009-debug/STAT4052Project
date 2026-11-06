# STAT4052Project
import Mathlib.Probability.Moments.SubGaussian
import Mathlib.Probability.Independence.Basic

open MeasureTheory
open ProbabilityTheory

section HoeffdingExample

-- Let Ω be a probability space, and X a random variable bounded almost everywhere in [a, b] with expectation zero.
-- Mathlib provides `hasSubgaussianMGF_of_mem_Icc` which proves that such a variable has a sub-Gaussian MGF 
-- with variance proxy c = (b - a)^2 / 4.

variable {Ω : Type*} [MeasurableSpace Ω] {μ : Measure Ω} [IsProbabilityMeasure μ]
variable (X : Ω → ℝ) (a b : ℝ)

-- Hoeffding's Lemma application:
-- If X ∈ [a, b] a.e. and expectation is 0, its moment-generating function is sub-Gaussian.
example (h_bound : ∀ᵐ ω ∂μ, X ω ∈ Set.Icc a b) (h_mean : ∫ ω, X ω ∂μ = 0) :
    HasSubgaussianMGF X ((b - a) ^ 2 / 4) μ :=
  hasSubgaussianMGF_of_mem_Icc h_bound h_mean

-- Hoeffding's Inequality for sums of independent bounded random variables:
-- Mathlib generalizes this via `HasSubgaussianMGF.sum_of_iIndepFun`, 
-- which derives tail bounds for sums of independent sub-Gaussian variables.

end HoeffdingExample
