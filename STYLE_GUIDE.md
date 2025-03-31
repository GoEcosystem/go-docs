# GoEcosystem Style Guide

This document outlines the coding standards and style guidelines for GoEcosystem projects.

## Go Code Style

1. Follow the [Effective Go](https://golang.org/doc/effective_go) guidelines
2. Use `gofmt` to format your code
3. Follow the [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

## Package Structure

1. Use small, focused packages
2. Group packages by domain rather than type
3. Avoid circular dependencies
4. Keep the `main` package as small as possible

## Error Handling

1. Error values should be returned as the last return value
2. Check errors immediately
3. Use descriptive error messages
4. Consider using wrapped errors (Go 1.13+)

## Documentation

1. Document all exported identifiers
2. Write clear, concise documentation
3. Include examples where appropriate
4. Keep documentation up-to-date with code changes
