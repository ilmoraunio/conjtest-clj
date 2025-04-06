# Changelog

This project uses [Break Versioning][breakver]. The version numbers follow a
`<major>.<minor>.<patch>` scheme with the following intent:

| Bump    | Intent                                                     |
| ------- | ---------------------------------------------------------- |
| `major` | Major breaking changes -- check the changelog for details. |
| `minor` | Minor breaking changes -- check the changelog for details. |
| `patch` | No breaking changes, ever!!                                |

`-SNAPSHOT` versions are preview versions for upcoming releases.

[breakver]: https://github.com/ptaoussanis/encore/blob/master/BREAK-VERSIONING.md

## 0.3.1-SNAPSHOT

## 0.3.0

- Fix failure-report for vector-based input [7e6520a](https://github.com/ilmoraunio/conjtest-clj/commit/7e6520a9c600417e487dc5959a9871ece8400725)
- Clarify failures for multiple messages [46873d2](https://github.com/ilmoraunio/conjtest-clj/commit/46873d2c2484f3554db92d02b15cbdbcf7fba8d2)
- Add corner test cases [cae20e6](https://github.com/ilmoraunio/conjtest-clj/commit/cae20e6b29528a294a0b87b5d4d18a53ce24884e)
- Run tests before release [54f1475](https://github.com/ilmoraunio/conjtest-clj/commit/54f147572fded9b29a74e8f2081c6658b1bbdad0)
- Clarify contract [95bf3c3](https://github.com/ilmoraunio/conjtest-clj/commit/95bf3c334298a0ffa4fcd943b5caa1094db2b563)
  - Breaking change: some internal functions are prefixed with `-`, eg: `-any-failures?`
- add docstrings [4c3982a](https://github.com/ilmoraunio/conjtest-clj/commit/4c3982acb7f3eef95d0d002576c457d2bac0970b)
- Add README [e7c39fb](https://github.com/ilmoraunio/conjtest-clj/commit/e7c39fb10851cf1d2e97de0076966f4197439c0e)

## 0.2.0

First release! 🎉
