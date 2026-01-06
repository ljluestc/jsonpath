# Support for Recursive Descent (..) Operator

## Description
This PR implements support for the recursive descent operator `..` (deep scan) in JSONPath, addressing issue #42.

## Changes
- **Tokenization**: Updated `tokenize` to correctly identify `..` as a distinct token instead of converting it to `*`. Added deduplication to handle cases like `....` gracefully.
- **Parsing**: Updated `parse_token` to recognize the `..` token and return a new `recursive` operation.
- **Execution**: Implemented `recursive` logic in the `Lookup` function.
  - Added `getAllDescendants` helper to traverse the object graph (Maps, Slices, Arrays).
  - Added lookahead logic to filter out Slice containers from recursive results when the subsequent step is a `key` lookup. This prevents duplicate matches (one from the container, one from the elements).
- **Testing**:
  - Added `TestRecursiveDescent` covering `$..author` and `$..price` scenarios.
  - Updated existing tests in `token_cases` within `jsonpath_test.go` to match the new tokenization behavior (expecting `..` instead of `*` for valid recursive queries).

## Verification
- Added new test case `TestRecursiveDescent` passes.
- All existing tests pass.

Fixes #42
