# OTA Review Checklist

## API necessity

- Is the API part of a real external contract?
- Is it only forwarding another API without adding meaningful behavior?
- Does it exist only for an obsolete compatibility path?

## Call-chain length

- Are there wrappers that only pass arguments through?
- Can responsibility be moved closer to the owning component?
- Would shortening the call chain reduce observability, compatibility, or error handling?

## Duplicate abstraction

- Are multiple helpers representing the same concept?
- Are multiple adapters normalizing the same data twice?

## State ownership

- Is state owned by one clear component?
- Are there duplicate caches or mirrored state transitions?

## Compatibility

- Is the interface consumed outside the repository?
- Is ABI/API compatibility required?
- Is a customer integration dependent on the current interface?

## Error handling

- Would simplification remove required error context?
- Would it change retry or rollback behavior?

## Test coverage

- Which tests prove the current behavior?
- What regression tests are needed before deletion?

## Rollback risk

- Can the change be reverted independently?
- Does it affect persistent OTA state or upgrade compatibility?

