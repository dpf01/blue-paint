# Paint Manifold: Color Perception Project

## Project Goals
The primary objective is to **quantify the subjective boundary** between two specific pigment anchors: **Phtalo Green** and **Cobalt Blue**.

Because "teal" is a perceptual battleground, this tool uses a scientific approach to map a user's **Perceptual Manifold**—the specific curve where their brain switches from seeing "Green" to seeing "Blue" across different lightness and darkness levels.

---

## Current Design & Logic

### 1. The Color Engine
*   **Pigment Anchors:**
    *   **Phtalo Green:** `rgb(0, 165, 80)`
    *   **Cobalt Blue:** `rgb(0, 71, 171)`
    *   **Black (Carbon):** `rgb(15, 15, 15)`
*   **Subtractive Pigment Model:** Simulates physical paint mixing by inverting RGB values to their CMY-like subtractive equivalents before processing.
    *   Calculation: `Final = 255 - (InvertedMix * (1 - white - black) + (InvertedBlack * black))`
*   **The 5-Tier Manifold:** The test evaluates color across five distinct lightness "tiers" to see if lighting/value shifts the user's perception:
    *   **Tint Max:** 45% White dilution.
    *   **Tint Mid:** 22% White dilution.
    *   **Pure:** 0% Dilution.
    *   **Shade Mid:** 35% Black concentration.
    *   **Shade Max:** 70% Black concentration.

### 2. The Precision Engine (Stochastic Balancer)
*   **Targeting Logic:** Uses a **Widest-Bound Targeting** algorithm. It prioritizes untested tiers, then identifies the tier with the highest uncertainty (`max - min`) for the next trial.
*   **Binary Search with Fuzzy Dampening:**
    *   **Hard Choice (Green/Blue):** Moves the boundary limit by **75%** toward the chosen ratio.
    *   **Unsure Choice:** Nudges both boundaries by **15%** to narrow the search space symmetrically.
*   **Termination:** The test concludes automatically once all 5 tiers reach a precision threshold of **$\le 5\%$ width**.

### 3. User Interface & Interaction
*   **Dynamic Immersion:** The entire page background updates to the current trial color to eliminate "surrounding color interference."
*   **Interactive History:**
    *   A "View Data" drawer tracks every trial.
    *   **Fuzzy Boundary Visualization:** Shows the current uncertainty range for each tier at the time of the trial.
    *   Users can **click any historical swatch** to apply that color as the page background for post-test inspection.
    *   Hovering over swatches reveals the exact blue-ward percentage.

### 4. Data Visualization & Insights
*   **The Manifold Chart:** A vertical bar chart where the Y-axis represents the Blue/Green threshold ($0\% = Green, 100\% = Blue$).
*   **Uncertainty Mapping:** Each bar features **Error Bars** (caps) representing the final remaining "fuzzy zone" for that tier.
*   **Drift Analysis:** Calculated by comparing the average threshold of "Tint" tiers vs. "Shade" tiers:
    *   **Shade-Drift:** Threshold is higher in shades (Perceiving darker tones as greener).
    *   **Tint-Drift:** Threshold is higher in tints (Perceiving lighter tones as greener).
    *   **Consistency:** Minimal difference ($< 4\%$) across all tiers.

### 5. Code structure
*   **Simple:** A simple website using plain HTML with embedded javascript and CSS. No need for javascript libraries or any backend.

---

## Key Variables for Context
*   **Target Precision:** 5%
*   **Trial Cap:** None (Variable based on user consistency).
*   **Stop Trigger:** User-initiated or auto-convergence.

