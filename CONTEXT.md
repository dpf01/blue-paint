# Paint Manifold: Color Perception Project

## Project Goals
The primary objective is to **quantify the subjective boundary** between two specific pigment anchors: **Phtalo Green** and **Cobalt Blue**.

Because "teal" is a perceptual battleground, this tool uses a scientific approach to map a user's **Perceptual Manifold**—the specific curve where their brain switches from seeing "Green" to seeing "Blue" across different lightness and darkness levels.

---

## Current Design & Logic

### 1. The Color Engine
*   **Subtractive Pigment Model:** Unlike standard RGB (light-based) mixing, the engine simulates physical paint mixing by inverting color values before processing.
*   **The 5-Tier Manifold:** The test evaluates color across five distinct lightness "tiers" to see if lighting/value shifts the user's perception:
    *   **Tint Max/Mid:** High white-pigment dilution.
    *   **Pure:** The base pigment mix.
    *   **Shade Mid/Max:** High black-pigment concentration.

### 2. The Precision Engine (Stochastic Balancer)
*   **Targeting Logic:** Instead of a fixed number of trials, the system uses a **Widest-Bound Targeting** algorithm. It identifies which of the 5 tiers has the highest current uncertainty and forces the next trial into that tier.
*   **Binary Search with Fuzzy Dampening:**
    *   **Hard Choice (Green/Blue):** Moves the boundary limit by **75%** toward the chosen ratio.
    *   **Unsure Choice:** Gently nudges both boundaries by **15%** to center the search space around the current color.
*   **Termination:** The test concludes automatically once all 5 tiers reach a precision threshold of **$\le 5\%$ width**.

### 3. User Interface & Interaction
*   **Dynamic Immersion:** The entire page background updates to the current trial color to eliminate "surrounding color interference."
*   **Interactive History:**
    *   A "View Data" drawer tracks every trial.
    *   Users can **click any historical swatch** to apply that color as the page background for post-test inspection.
    *   Hovering over swatches reveals the exact blue-ward percentage.

### 4. Data Visualization & Insights
*   **The Manifold Chart:** A vertical bar chart where the Y-axis represents the Blue/Green threshold ($0\% = Green, 100\% = Blue$).
*   **Uncertainty Mapping:** Each bar features **Error Bars** (caps) representing the final remaining "fuzzy zone" for that tier.
*   **Drift Analysis:** The system automatically calculates and explains:
    *   **Shade-Drift:** Perceiving darker tones as greener.
    *   **Tint-Drift:** Perceiving lighter tones as greener.
    *   **Consistency:** A flat manifold across all tiers.

### 5. Code structure
*   **Simple:** A simple website using plain HTML with embedded javascript and CSS. No need for javascript libraries or any backend.

---

## Key Variables for Context
*   **Target Precision:** 5%
*   **Trial Cap:** None (Variable based on user consistency).
*   **Stop Trigger:** User-initiated or auto-convergence.

