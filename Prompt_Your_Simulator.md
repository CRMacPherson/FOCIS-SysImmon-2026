# **Building the Tumor Microenvironment ABM: A Step-by-Step Guide**

Use the following series of prompts in a Large Language Model (like Gemini) to iteratively build the Agent-Based Model (ABM) simulator. By building the app in stages, you will understand how physical collision rules emerge into complex biological data.

### **Prompt 1: The Core Physics and Visual Simulator**

*Goal: Establish the canvas, the agents, their morphologies, and the rules of interaction.*

**Copy and paste the following prompt:**

"I want to build a web-based Agent-Based Model (ABM) simulating a tumor microenvironment using vanilla JavaScript, HTML5 Canvas, and Tailwind CSS.

Please create a simulation with three types of agents:

1. **Tumor Cells:** Stationary, colored amber. They should have a radius of 8\.  
2. **CD8+ T-Cells:** Moving randomly, colored blue. They have an 'active' state.  
3. **Treg Cells:** Moving randomly, colored red.

**Visuals:** Make the cells look organic rather than perfect circles by using a trigonometric perturbation function to draw irregular, slightly undulating membranes. Give each cell a darker 'nucleus'.

**Interaction Rules (Collisions):**

* If an *active* CD8+ cell overlaps with a Tumor cell, there is a 3% chance per frame P(Kill) that the tumor cell is destroyed.  
* If a Treg cell overlaps with an *active* CD8+ cell, there is a 20% chance per frame P(Suppress) that the CD8+ cell is permanently suppressed (turn its color to gray and it can no longer kill tumors).

Include a 'Run Visual Simulation' button to start the animation and text counters showing the current number of each cell type. For now, hardcode it to start with 100 tumors, and generate CD8+ and Treg cells at a constant rate of 0.20 and 0.10 cells per frame, respectively."

### **Prompt 2: Parameterization and UI Controls**

*Goal: Turn the hardcoded sandbox into an interactive tool by adding adjustable parameters.*

**Copy and paste the following prompt:**

"This looks great. Now, I want to make the simulation configurable. Please add a left sidebar with input fields to control the following parameters:

* Initial Tumor Cells (default 100\)  
* CD8+ Rate (cells generated per frame, default 0.20)  
* Treg Rate (cells generated per frame, default 0.10)  
* P(Kill) per contact (default 0.03)  
* P(Suppress) per contact (default 0.20)  
* Max Simulation Frames (default 800\)  
* Responder Threshold (default 25 tumors)

The simulation should end automatically if it reaches the 'Max Simulation Frames' or if all tumors are destroyed. When the user clicks 'Run Visual Simulation', it should pull the values directly from these input fields."

### **Prompt 3: The Data Pipeline and Batch Generation**

*Goal: Transform the visual simulator into a synthetic data generator for statistical analysis.*

**Copy and paste the following prompt:**

"Now I want to use this simulator to generate a synthetic cohort dataset.

1. **Randomization Toggles:** Next to every parameter input in the sidebar, add a 'Rnd' (Random) checkbox. When checked, reveal a second input field to define a 'Max' value. The simulation should uniformly sample a random number between the min and max values just before it runs.  
2. **Batch Generation:** Add a 'Generate 100 Patients (Batch)' button. When clicked, it should run the simulation logic 100 times in the background (without animating the canvas to save time), sampling new random parameters for each run if the toggles are checked.  
3. **Data Table:** Add a scrollable data table below the canvas. For every completed simulation (visual or batch), add a row recording: Patient ID, CD8 Rate, Treg Rate, the Ratio of CD8/Treg rates, Final Tumor Count, and a Clinical Response label ('Response' if Final Tumors \<= Responder Threshold, else 'No Response').  
4. **Export:** Add a 'Download CSV' button in the header that exports the generated table data so I can analyze the statistical correlations between the starting parameters and the final clinical response."