# opentracing-java-thrift

Spond's fork of [opentracing-contrib/java-thrift](https://github.com/opentracing-contrib/java-thrift): OpenTracing instrumentation for Apache Thrift clients and servers, published as `com.spond:opentracing-thrift` and consumed by Spond's Thrift services.

## Prerequisites

- JDK 17. The pom compiles for Java 7, which JDK 20+ can no longer target.
- The `thrift` compiler on `PATH`: the build generates test sources from `src/test/thrift/`.

## Commands

- Build and test: `./mvnw -B verify`
- Single test: `./mvnw -B test -Dtest=TracingTest#withError`

There is no linter or formatter; follow the surrounding style (two-space indent, Java 7 syntax only, so no lambdas or streams).

## Conventions

- Span names and tags follow Spond's tracing conventions, and every Thrift service's traces depend on them. Client spans are `thrift.call` and server spans `thrift.operation`, with the Thrift method in the `resource.name` tag. Don't rename them, and keep the assertions in `TracingTest` in line with them.
- `libthrift` is `provided`: consumers bring their own Thrift version.
- New source files carry the Apache header from `header.txt`. Nothing enforces it, so reviewers check by hand.
- Bump `<version>` in `pom.xml` in any change that is meant to be published. Releases are plain versions, not `-SNAPSHOT`.
- Commits and PR titles use Conventional Commits. PR titles include the Linear ID: `fix: MAC-123 <subject>`.

## Releasing

Publishing to AWS CodeArtifact is manual and done by a human. Agents stop at a green build with the version bumped. The command is `./mvnw -B -s .settings.xml deploy -DskipTests`, with `CODEARTIFACT_REPO` and `CODEARTIFACT_AUTH_TOKEN` set. CI only builds and tests.

## Upstream

`upstream` is the original opentracing-contrib repo. Never open PRs or push there. Keep Spond changes small and local so merging from upstream stays easy.

## Don't touch

- `mvnw`, `mvnw.cmd`, `.mvn/`: generated Maven wrapper.
- `CODEOWNERS`.
- `.settings.xml`: only the `codeartifact` server is in use. The other entries are upstream leftovers.
