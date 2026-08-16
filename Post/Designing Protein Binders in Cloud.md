# Designing Protein Binders in the Cloud: What ESMFold2 and Modal Make Possible

One of the quieter shifts happening in drug discovery right now isn't a new drug class — it's a change in *where* the design work happens. Increasingly, the first draft of a therapeutic molecule is built not on a lab bench, but inside a cloud computing job, using a protein language model to propose a structure before a single reagent is touched.

At AlgorithmicRx, we spend a lot of time evaluating tools like this, because the computational front-end of drug discovery is exactly where we operate. One workflow worth walking through is a recent approach built around **ESMFold2**, a biological language model that predicts protein structure, paired with **Modal**, a cloud platform built for running large numbers of compute-heavy jobs in parallel. Together, they're being used to design **binders** — small proteins engineered to latch onto a specific target, such as an immune checkpoint protein or a marker found on cancer cells.

## Why This Needs Cloud Infrastructure at All

Protein design sounds like it should be a modeling exercise, but in practice it's a computational bottleneck. Two problems make it hard to do on a laptop or even a single workstation:

**It's GPU-hungry.** Predicting how a protein folds — and testing whether a candidate binder will actually sit against its target the way it's supposed to — requires a high-end graphics card for every single design.

**It's a numbers game.** A single design attempt rarely produces a usable binder. Getting to a handful of strong candidates typically means generating and evaluating hundreds of variants, known as "trajectories." Run those sequentially, and a project that should take a day stretches into weeks.

Modal addresses both problems by letting hundreds of these GPU jobs run simultaneously in the cloud, rather than one after another. The practical upshot is that a researcher can kick off a large batch of design attempts and walk away — the infrastructure scales to meet the job instead of the job waiting on the infrastructure.

## Inside the Workflow

The process generally moves through four stages:

**1. Setup.** The researcher connects their environment to a Modal account and installs the tooling needed to run ESMFold2-based design jobs.

**2. A sanity check, first.** Before committing serious compute, a single design job is run against a known target — CTLA-4, an immune checkpoint protein, is a common built-in choice — or against a custom protein sequence supplied by the researcher. The goal here isn't a finished binder; it's confirming that the system is producing a plausible, well-formed 3D structure before scaling up.

**3. The sweep.** Once the setup checks out, the real run begins: often hundreds of design jobs launched at once. Because everything executes remotely, the researcher doesn't need to keep a machine running or babysit the process — the cloud jobs finish independently.

**4. Selection.** With hundreds of candidate designs in hand, the workflow filters them down. Designs that would be impractical to work with experimentally — poor solubility, for instance — are screened out automatically, and the remainder are ranked by how confidently the model predicts they'll bind the target.

## What Comes Out the Other End

The output of this process is, deliberately, lab-ready rather than purely theoretical:

- **A shortlist of top-ranked sequences** — often around 84 candidates, a batch size that maps well onto standard laboratory testing formats.
- **3D structural models** showing how each candidate binder is predicted to dock against its target, giving researchers a visual, mechanistic hypothesis before any wet-lab work begins.

## Why This Matters Beyond the Notebook

What's notable about this kind of workflow isn't any single technical trick — it's what it represents: computational tools that used to require dedicated infrastructure teams are becoming accessible enough for a focused research group to run directly. That shift matters most for problems that have historically been underserved by exactly that kind of infrastructure — rare diseases, where patient populations are too small to justify the scale of investment large pharma applies to common conditions.

This is squarely the space AlgorithmicRx works in. We're not the developers of this particular ESMFold2/Modal workflow, and we want to be clear about that — but tools like it are part of the broader landscape of open protein-modeling infrastructure we track and evaluate as part of our own in silico target discovery work. As that infrastructure keeps getting more capable and more accessible, it becomes more feasible for smaller, mission-driven teams to do serious structural biology work without needing the compute budget of a large pharmaceutical company.

We'll continue writing about tools and methods like this as we evaluate them — not as endorsements, but as part of an honest account of what the current computational drug discovery toolkit can and can't yet do.
