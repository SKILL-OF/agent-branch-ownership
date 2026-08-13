# SKILL-OF/agent-branch-ownership

The grammar for encoding branch lineage in the filesystem and deriving an
agent's operational role from its working directory path.

## BIAFRAL

**B**orn **I**n **A** **F**ilesystem **R**ecursing **A**gentic **L**ineages.

An agent's working directory path IS its operational context. The depth and
structure of the `_/AS/` chain above the gitroot determines the agent's role,
its integration obligations, and the branch history it must respect — without
reading any config file or receiving any explicit instruction.

## The 4-register grammar

```
1  org      <ghorg>          remote organization
2  repo     <repo>           remote repository; also the main copy's folder name by convention
3  copy     <name>           local working copy or linked worktree; disk name = git worktree name
4  branch   <branch>         current HEAD; encoding in path makes it a contract
```

With AS casting:

```
_/AS/<org>/_/AS/<repo>/                                  registers 1+2; copy=main, branch=movable
_/AS/<org>/_/AS/worktree/_/AS/<name>/                    registers 1+2+3; type=locked
_/AS/<org>/_/AS/worktree/_/AS/<name>/_/AS/<branch>/      all 4; explicit branch contract
```

## The start-point register (ephemeral provenance)

When a worktree or branch is created:

```bash
git worktree add -b feature/thing ../copy main
#                                          ^^^^
#                                          start-point: the branch this was created from
```

Git does not store this relationship durably. The path encodes it:

```
_/AS/main/_/AS/worktree/_/AS/copy/_/AS/feature/thing/
   ^^^^
   start-point preserved in path; only durable record
```

The start-point is the nearest required integration upstream for this copy.

## The integration chain (depth mode)

For complex branch hierarchies, the full chain is encoded by nesting:

```
_/AS/main/
  _/AS/devline/
    _/AS/sprint-25-integration/
      _/AS/feature/navigation/
        _/AS/feat-nav-ui-integration/
          _/AS/task-connect-UI-to-provider-state/
            _/AS/<copy>/    ← agent's gitroot; path encodes every upstream obligation
```

Each `_/AS/<branch>/` segment is a level at which other developers are actively
working. An agent born at the leaf knows it must integrate from every ancestor.

## Worker and integrator roles

**Worker** (breadth mode) — born at a leaf with one visible upstream:

```
_/AS/feat-nav-ui-integration/_/AS/<copy>/_/AS/task-connect-UI-to-provider-state/
```

The agent sees one integration obligation. It does not need the full chain.

**Integrator** (depth mode) — born at a node with the full chain in the overpath:

```
_/AS/main/_/AS/devline/_/AS/sprint-25/_/AS/feature/navigation/_/AS/<copy>/
```

The agent reads its entire obligation set by walking upward through its own
path. No configuration file or explicit instruction needed.

**The path IS the role assignment.** The orchestrating agent controls role by
controlling how deep a path it spawns the worker into.

## Breadth mode (peer copies)

When agents only need immediate-upstream context, peer copies suffice:

```
_/AS/branch1/_/AS/copy1/_/AS/branch2/
_/AS/branch2/_/AS/copy2/_/AS/branch3/
_/AS/branch3/_/AS/copy3/_/AS/branch4/
```

Each copy only knows its nearest upstream. This is sufficient when integration
decisions are made outside each copy's scope.

## Relationship to upstream tracking branch

Git stores the remote tracking branch (`branch.<name>.remote` + `.merge`). This
is derivable from git state and does not need its own path register. The
start-point (what the branch was created from locally) is NOT stored by git and
is the register the path uniquely preserves.
