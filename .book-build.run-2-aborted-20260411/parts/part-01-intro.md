# Part I: Foundations and Framing

Every book about a complex system must begin by answering two questions before the reader will grant it any patience: why does this system matter, and how should I approach understanding it? Part I answers both.

The theme of this Part is orientation. Before descending into the machinery of a 400,000-line codebase, the reader needs a thesis that explains what they are looking at, a map that shows where everything lives, and a grasp of the unconventional technology choices that make the whole thing possible. These four chapters deliver exactly that sequence.

Chapter 1, "Why This Book Exists: The Harness Engineering Moment," positions Claude Code (cc) within the emerging discipline of harness engineering -- the craft of building the deterministic scaffolding around a language model that turns raw inference into trustworthy autonomous behavior. The chapter argues that cc is the most mature public example of such a harness and therefore a template for anyone building long-running agent systems.

Chapter 2, "Book Architecture, Reading Paths, and Citation Conventions," lays out how this book is organized, the notation it uses for cross-references, and the conventions governing diagrams and source citations. It also provides a mapping table that connects the twelve patterns, seventeen failure modes, and twelve best practices from the Harness Engineering Reference (HER) to the chapters where each appears, enabling non-linear reading paths for practitioners who want to jump straight to a specific concern.

Chapter 3, "A Guided Tour of the Repository," walks the top-level directory tree of the cc codebase. For every major folder it answers three questions: what code lives here, why it lives here rather than elsewhere, and what other folders it depends on. This chapter is the reader's orientation map before any deep dive, and it maps the HER reference architecture's eight layers onto the concrete directory structure.

Chapter 4, "TypeScript, Bun, React, Ink: The Unusual Runtime Stack," explains why cc runs on TypeScript with the Bun runtime, renders its terminal interface through React and Ink, and uses feature-flagged bundling for distribution. These choices are not arbitrary -- they reflect deliberate tradeoffs around startup latency, structured UI rendering in a terminal, and the broader thesis that harnesses are language- and runtime-specific artifacts where the choice of substrate shapes every subsequent architectural decision.

These chapters belong together because they form a single conceptual arc: from the external motivation (why this matters), through the reading contract (how the book works), to the physical terrain (where the code lives), and finally to the material foundation (what the code is made of). Each chapter depends on the ones before it, and together they establish the shared vocabulary and mental model that every subsequent chapter assumes.

By the end of Part I, the reader should be able to articulate the book's central thesis, navigate the repository without disorientation, and explain why cc's runtime stack is what it is rather than something more conventional. These are the prerequisites for everything that follows -- the bootstrap sequence, the query loop, the tool system, and beyond.

The table of contents in the front matter provides the full chapter listing and page numbers for the entire book.
