# Narrow the Scope

Techniques for isolating the problematic code:

## Binary Search Through Files

1. Identify last known working state
2. Check midpoint file between working and broken
3. Narrow range based on results
4. Repeat until cause isolated

## Isolate Components

1. Comment out recent changes one at a time
2. Test after each removal
3. When issue disappears → found the cause

## Remove Variables

When multiple things changed:
1. Remove half, test
2. Narrow to quarter
3. Continue until single cause identified

## Layer Isolation

Work from outer to inner layers:
1. Frontend/API boundary
2. API/Database boundary
3. Configuration/Code boundary
4. Environment/Code boundary
