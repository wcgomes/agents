# Verification Techniques

How to properly verify hypotheses:

## Falsifiable Tests

A good verification:
- "Returns error X" — proves hypothesis wrong
- "File contains Y" — confirms state
- "Command outputs Z" — verifies behavior

A bad verification:
- "Seems right" — subjective
- "No obvious errors" — not specific
- "Probably works" — not falsifiable

## Quick Wins

Before spending time on complex verification:
1. Read error message fully
2. Check file exists
3. Check spelling/typos
4. Check import paths
5. Check environment variables

## Logging Strategies

When behavior is unclear:

1. Add minimal logging at decision points
2. Run scenario with logging
3. Analyze trace — path taken vs expected path
4. Remove logging after issue found

## Reproduction Cases

Create minimal case that reproduces issue:

1. Strip everything not needed
2. Keep only inputs and expected behavior
3. Test repeatedly
4. Fix confirmed, then test in full context
