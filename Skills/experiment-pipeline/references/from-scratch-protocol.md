# From-Scratch Reproduction Protocol

Activated when no usable source code exists for a baseline (determined by Source Code Audit). This protocol replaces the standard Stage 1 process with a structured knowledge acquisition → specification → milestone → execution flow.

## Step 0: Source Code Audit

Before any implementation, determine which implementation mode applies:

1. Use `local-paper-navigator`'s `find_code.py --title <title>` to search online (GitHub + HuggingFace)
2. Use `local-paper-navigator`'s `code_repo_search.py --query <keyword>` to search local code-repo (`workspace/code-repo/`)
3. Classify each baseline:

| Mode | Condition | Budget | Example |
|------|-----------|--------|---------|
| Adapt | Usable code found (local or online) | ≤20 | Paper has GitHub repo |
| From-Scratch | No usable code anywhere | ≤30-40 | GFlowNet from paper description |
| Hybrid | Partial code exists | ≤25 | Core algo available, data pipeline missing |

4. Write audit to `/experiments/stage1_baseline/source-code-audit.md` (use `assets/source-code-audit-template.md`)

## Step 1: Implementation Specification Extraction

Read the anchor paper at L1 level via `local-paper-navigator`'s `fetch_paper.py --paper-id <ID> --reading-level L1 --full-stdout` and produce `implementation-spec.md` (use `assets/implementation-spec-template.md`):

Extract from the paper:
- **Algorithm Core**: key equations → pseudocode → data flow diagram
- **Architecture Details**: network structure, state space, input/output dimensions. Mark `[IMPLICIT]` for anything the paper doesn't specify
- **Data Pipeline**: feature definition complete list, preprocessing step exact order and parameters, expected statistics for verification
- **Training Specification**: loss function exact formula, optimizer + schedule, batch construction strategy, training duration

**[IMPLICIT] markers** are details the paper omits but implementation requires. Each gets a resolution level:
- **L1**: Infer from paper's other sections → re-read the paper
- **L2**: Obtain from method's original publications → search via `local-paper-navigator`
- **L3**: Domain engineering practice → search tutorials/community via WebSearch
- **L4**: Empirical verification → design small-scale validation experiments

## Step 2: Implementation Milestone Planning

Break the full implementation into independently verifiable milestones (each ≤1 day of work):

| Milestone | Scope | Mini-Gate | LOC Est |
|-----------|-------|-----------|---------|
| M1: Data pipeline | Feature computation + preprocessing | Computed feature stats match paper values | ~200-400 |
| M2: Algorithm skeleton | State space + policy construction | Can generate valid trajectories | ~400-800 |
| M3: Training loop | Loss computation + optimizer step | Loss decreases over 100 steps | ~200-300 |
| M4: Evaluation | Metric computation + result comparison | IC computation produces reasonable values | ~100-200 |

Execute M1→M2→M3→M4 sequentially. Each milestone follows the same generate → execute → record → diagnose → revise loop, with its own mini-gate. **Do not skip milestones.** Each unverified milestone compounds debugging difficulty in subsequent milestones.

## Step 3: Knowledge Gap Resolution

For each `[IMPLICIT]` marker, create `knowledge-gap-resolution.md`:
- **L1/L2**: Use `local-paper-navigator` to read relevant papers (the method's original paper, related works)
- **L3**: Use WebSearch for community implementations, engineering guides, and tutorials
- **L4**: Design small-scale validation experiments (e.g., test one hyperparameter on synthetic data)

## Step 4: Budget Adjustment

From-Scratch implementations require higher budgets (see `references/attempt-budget-guide.md`):
- Stage 1 budget: increase from ≤20 to ≤30-40
- Total pipeline cap: increase from 62 to 82-92 maximum
- Estimated LOC: multiply paper's claim by 5-10× (papers typically report only core modification LOC)
- Estimated time: multiply paper's claim by 2-3×

## Common From-Scratch Pitfalls

| Pitfall | Symptom | Fix |
|---------|---------|-----|
| Architecture details not in paper | Can't build policy network | Search method's original paper + community; start simplest |
| Numerical stability tricks omitted | Training diverges | Search community for standard tricks; add one per attempt |
| LOC far exceeds paper estimate | 200 LOC claim = 2500+ reality | Plan milestones; never implement all at once |
| Paper describes concept but omits engineering | Core idea clear but can't run it | Extract specification first; fill [IMPLICIT] gaps before coding |
