## fix(loader): recover file-sourced imports after the imported file is moved

Fixes #112

### Root cause

When importing a node from a local file, the editor stores the file's path **relative to the importing flow at import time** (`{ type: "file", data: relativePath }`, see `scan-importable-nodes.ts`). The loader resolves it as `join(fullFlowPath, "..", source.data)` (`findReferencedNodeServer.ts`). If the imported file is later moved to another folder, the stored path no longer resolves and the flow fails to load with `Cannot find node <id> in <path>`.

### What this PR does

- Adds `recoverMovedFileSource()` (`loader/src/resolver/server/recoverMovedFileSource.ts`): when the stored path is broken, it locates the project root (nearest `package.json`/`.git`) and searches it for files sharing the stored basename, preferring the candidate whose path suffix best matches the stored relative path.
- Wires it into the `case "file"` branch of `createServerReferencedNodeFinder` **only as a fallback**: direct hits keep the exact previous behavior, and fully unresolvable cases still throw the original error. Healthy flows see zero changes.
- No new dependencies. The search skips `node_modules`, `.git`, `dist`, etc., and is capped in depth (8) and results (50) to stay fast on large projects.
- Adds `recoverMovedFileSource.spec.ts` with 5 hermetic mocha cases (direct hit, recovery after move, node_modules exclusion, no-candidate → null, multiple candidates).

### Verification

- `npm test` in `loader/` runs the new spec (mocha, same stack as existing specs).
- The logic was additionally validated standalone against the scenarios from the issue report (direct import → move file → flow still resolves).
