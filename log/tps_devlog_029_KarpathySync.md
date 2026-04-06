# TPS Devlog 029: Karpathy Validates the TPS Centaur Team Workflow

**Date:** Apr 5, 2026
**Status:**  🔥 
**Mood:** 💪🤖🌀 

Andrej Karpathy (@karpathy) remains one of the true heavyweights in the AI field — the kind of mind that doesn’t just ride the wave but quietly rewrites the surfboard while everyone else is still arguing about the shape. Former Director of AI at Tesla, founding member of OpenAI, and the guy who’s been dropping signal on LLMs since before most people knew what a transformer was. When he speaks on workflows, the entire field leans in.

## Karpathy’s “LLM Knowledge Bases” Workflow (Direct from His April 2 Post)

In his [latest viral X post](https://x.com/karpathy/status/2039805659525644595?s=46&t=FcKma2BPH-2Y51–ij62WA), Karpathy outlines a dead-simple yet insanely powerful pattern he’s been using for personal research:

- **Data Ingest**: Dump raw sources (articles, papers, repos, datasets, images, web clips) into a `raw/` directory. Use tools like Obsidian Web Clipper for clean Markdown + local image downloads.
- **Compilation Layer**: Feed everything to an LLM (with a clear schema) to incrementally “compile” a living wiki — a clean directory of interlinked `.md` files. The LLM writes summaries, backlinks, concept articles, and maintains the entire structure.
- **IDE**: Obsidian as the lightweight frontend. You rarely edit the wiki manually — the LLM owns it.
- **Q&A & Operations**: Once the wiki hits critical mass (~100 articles / 400K words in his case), query the LLM against the full structure. It auto-researches, cross-references, and returns outputs as new Markdown files, Marp slides, matplotlib visuals, etc.
- **Linting & Maintenance**: Run periodic LLM “health checks” to catch inconsistencies, impute missing data, surface new connections, and propose fresh articles. Every query and exploration feeds back into the wiki — it compounds.
- **Future Horizon**: Synthetic data generation + fine-tuning so the knowledge eventually lives in the model weights, not just the context window.

**TL;DR from Karpathy himself:**  
> “raw data … is collected, then compiled by an LLM into a .md wiki, then operated on by various CLIs by the LLM to do Q&A and to incrementally enhance the wiki … You rarely ever write or edit the wiki manually, it’s the domain of the LLM.”

Pure folder + Markdown + LLM-as-maintainer. No fancy databases. No locked-in apps. Just files that get smarter every time you touch them.

## How TPS Nailed the Exact Same Loop — Months Ago
We didn’t wait for Karpathy’s post.

Back in **February 2026** (first commits landed Feb 3), we fed the raw Abhidhamma canon, kernel specs, thermodynamic alignment papers, simulation-debugging notes, financial logs, and every scrap of Centaur Team synthesis straight into **NotebookLM + our multi-LLM Centaur Team (Gemini + Grok + DeepSeek)**.

The result?  
- A production-grade, git-versioned Markdown wiki living at `https://github.com/c4chaos-io/trillions-per-second`  
- `src/alignment/`, `ARCHITECTURE.md`, layered directories, backlinks, concept articles, and the full Silicon Sutra White Paper all maintained by the same raw → compile → enhance loop Karpathy just described.  
- Every new insight, podcast episode, or real-time log gets synthesized back in under the White Hat Protocol. The repo *literally* describes itself as a “recursive fractal of the original algorithm.”

We even added the missing audio dimension: the **TPS Companion Podcast** (NotebookLM’s Audio Overviews turned into an ongoing public series) — Layer 1.5 of the exact same knowledge base.

No Obsidian frontend required (though you can open the repo in it instantly).  
No private cloud.  
Fully forkable, version-controlled, and already compounding in public under CC-BY-SA 4.0.

Karpathy just dropped the canonical description of a workflow we had running in production **two months earlier** — while translating 2,500-year-old Abhidhamma into modern RL-TA kernel specs and debugging the Self Rootkit at scale.

This is not coincidence.  
This is the simulation recognizing its own debug tools when they appear.

The Centaur Team remains on watch.  
The wiki is alive.  
The podcast is the voice layer.  
And the compounding continues.

The manual for hacking the simulation just got external validation from one of the heaviest hitters in the field.

## The Difference

The biggest differences between the TPS Centaur Team's workflow from Karpathy's LLM Knowledge Bases are:

- Karpathy's LLM KB are private vaults. TPS is an Open Source forkable GitHub repo. 

- Karpathy's workflow is fully automated by AI. In TPS, the lead developer is a human working together with AIs. The human manually generates the markdown files (Chapters, Devlogs, podcast episodes), tests, and pushes the updates to the GitHub repo. TPS is still foremost a book project that eventually blew up into a living knowledge base.

- TPS has a companion podcast as another mode of information presentation. It's like an audiobook on steroids wherein the source materials episode are the TPS main contents as well as the history of project development via the devlogs. The podcast episodes are manually generated by the lead author using a companion NotebookLM knowledgebase containing the history of the TPS podcast. 

In summary, TPS serves as a proof of concept that a Centaur Team approach is the more hardcore and optimal workflow on hacking the simulation, at least until the advent of ASI, maybe 😅

**Listen to the TPS Podcast for the deep dive.**

**📺[TPS Pod Ep. 38: Karpathy Valides the TPS Centaur Team Worklfow](https://youtu.be/dwBt1ESzh3E?si=LGzNjHtXQEJEqcKP)**


