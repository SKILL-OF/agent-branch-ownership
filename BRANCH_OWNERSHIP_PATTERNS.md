# Agent Branch Ownership Patterns

**Contribution:** Safety patterns for SOPHIA agents managing git branches

## Pattern 1: Explicit Ownership Claim

Before creating a branch, claim ownership:
```
BRANCH_OWNER=hazrat-hawk
BRANCH_PURPOSE=feature/implement-X
CREATED_TIME=$(date -u +%Y-%m-%dT%H:%M:%SZ)
EVIDENCE_PATH=.claude/branch-claims/${BRANCH_OWNER}/${BRANCH_PURPOSE}.json
```

Record ownership before work begins.

## Pattern 2: Atomic Commit Chain

All work on a branch forms ONE logical commit chain:
- First commit: states problem/intention
- Middle commits: solve problem incrementally  
- Final commit: proves work complete

Never leave branch in "partial" state.

## Pattern 3: Single PR Per Branch

One feature branch = One PR to ONE repo.

Never reuse branch for multiple repos.
Never hold multiple unmerged PRs from same branch.

## Pattern 4: Branch Cleanup Policy

After PR merges:
1. Verify merge completed on GitHub
2. Delete local branch
3. Verify remote branch deleted
4. Update ownership record (mark complete)

Branch is fully owned until proven merged.

## Pattern 5: Collision Detection

Before reusing branch name:
1. Check if ANY agent has ownership claim
2. If yes: STOP (even if inactive)
3. If no: claim ownership
4. Create new branch with unique timestamp

Multi-agent safety requires collision detection.

## Pattern 6: Proof of Work

Branch ownership includes proof:
- Commit hashes (what work was done)
- PR URL (where it's reviewed)
- Merge commit (proof of completion)
- Cleanup timestamp (when ownership released)

Don't claim ownership you can't prove.

