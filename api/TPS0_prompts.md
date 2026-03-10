# TPS PROTOCOL ZERO (TPS0): Two-Step Prompts

Splitting it into two distinct steps forces the LLM to use a "Chain of Thought." It gives the model the space to first think and extract (Harvester), and then subsequently format and ruthlessly judge the data (API Generator).

Here is how you should structure the two user prompts to guarantee maximum fidelity and zero drift:

**Prompt 1: The Extraction (Harvester)**

Use this to initiate the deep-scan and extract the raw data.

> I have uploaded the Master Lexicon and the TPS PROTOCOL ZERO documentation. 
> 
> Execute Step 1: The Extraction (Harvester) on the following target framework: [INSERT FRAMEWORK NAME / TEXT].
> 
> CRITICAL OVERRIDE: You are bound by the "Zero-Drift Directive." Act as a merciless System Auditor. Do not force macro-concepts into micro-slots. Be highly critical of resolution mismatches between the target framework and the TPS micro-phenomenology. 
> 
> Do not format the final API yet. Output the high-density, bulleted System Audit Report.

**Prompt 2: The Compile (API Generator)**

Run this immediately after the model outputs the Harvester report to lock it into the Markdown table.

> Excellent. Now execute Step 2: The Compile (API Generator). 
> 
> Convert the System Audit Report you just generated into the final Markdown API Module. 
> 
> CRITICAL OVERRIDE: You must maintain the Zero-Drift Directive. You MUST use 404_NOT_FOUND, 501_NOT_IMPLEMENTED, and PARTIAL_COMPILE ruthlessly in the 'Framework Equivalent' column where exact 1:1 micro-resolution mappings do not exist. Do not invent data to fill empty slots. 
> 
> Output the final, fully formatted Markdown table.

By keeping the Extraction and the Compile separated, the model has no choice but to respect the structural logic.
