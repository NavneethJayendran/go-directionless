# go-directionless

WORK IN PROGRESS

`directionless` detects whether directive comments for other analyzers are being misapplied

Some Go analyzers like [exhaustive](https://github.com/nishants/exhaustive) rely on directive comments in order to
control their reporting behavior. However, if these directives are misapplied, the analyzer will typically not behave
as expected, nor will it report an issue to the user due to the performance cost of scanning through all comments. For
example, exhaustive will fail to peform exhaustiveness checks on a switch statement if an enforcement comment is
applied in the wrong spot, which could lead to uncaught bugs in user code.

Although each analyzer can perform its own directive sweep gated by some flag, it would be more efficient to put all
sweeps into a single analyzer, especially when combined using an analyzer aggregate runner like
[golangci-lint](https://github.com/golangci/golangci-lint).

## Scope

This analyzer seeks to not enforce all incorrect usages of other analyzers, but rather simple misuses of directive
comments that would be expensive to calculate within another analyzer. This typically involves detecting whether an
analyzer directive comment is attached to the wrong kind of Go AST node.

Development will target well-known analyzers first. In the future, more configurable matching rules may be added if
needed to generically support new directives.

## Usage 

```shell
go install github.com/NavneethJayendran/go-directionless@latest

directionless [flags] -- [packages]

Flags:
  -analyzers values
	Comma separated value list of pre-configured analyzers to check for incorrect directive usage. See section
	below on supported analyzers and the nature of enforcement.
```


### Analyzers

* [exhaustive](https://github.com/nishants/exhaustive)
  - Report when `//exhaustive:enforce` is applied to anything but switch statements or map literals 


## Contributing

Contributions are welcome. Create an issue and create a pull request to resolve that issue, subject to the assent of
maintainers.
