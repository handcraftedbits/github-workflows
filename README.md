# github-workflows

Common GitHub Workflows.

Reusable workflows live in [`.github/workflows`](.github/workflows); a ready-to-copy caller for each one lives in
[`examples`](examples).

# maven-publish

Runs `mvn -Prelease deploy` against a project using the
[HandcraftedBits parent POM](https://github.com/handcraftedbits/handcraftedbits-parent), publishing to
[Maven Central](https://central.sonatype.com) through the Central Publisher Portal.

## Usage

* Pushing to the `development` branch will publish a `SNAPSHOT` version.
* Pushing a tag named `release/x.y.z` will publish version `x.y.z`.

See [`examples/maven-publish.yml`](examples/maven-publish.yml) for a complete caller including triggers.

## Inputs

| Name                | Default          | Purpose                                                                    |
|---------------------|------------------|----------------------------------------------------------------------------|
| `environment`       | (none)           | GitHub environment to run in, for required reviewers or scoped secrets.    |
| `java-distribution` | `temurin`        | JDK distribution passed to `actions/setup-java`.                           |
| `java-version`      | `25`             | Java version to build with.                                                |
| `maven-args`        | (empty)          | Extra arguments appended to the Maven command line.                        |
| `ref`               | the caller's ref | Ref to check out.  Must be a `release/*` tag to produce a release version. |
| `runs-on`           | `ubuntu-latest`  | Runner label for the job.                                                  |
| `timeout-minutes`   | `30`             | Job timeout.                                                               |
| `working-directory` | `.`              | Directory containing the root `pom.xml`.                                   |

## Secrets

| Name                     | Required | Purpose                                                                                                           |
|--------------------------|----------|-------------------------------------------------------------------------------------------------------------------|
| `GPG_PASSPHRASE`         | no       | Passphrase for that key; omit if it has none.                                                                     |
| `GPG_PRIVATE_KEY`        | yes      | ASCII-armored private key used to sign artifacts (`gpg --armor --export-secret-keys <key-id>`).                   |
| `MAVEN_CENTRAL_USERNAME` | yes      | Central Portal user token username, generated at https://central.sonatype.com/account.  Not the account password. |
| `MAVEN_CENTRAL_PASSWORD` | yes      | Central Portal user token password.                                                                               |

`secrets: inherit` works as well if the names match.

## Outputs

| Name      | Purpose                                |
|-----------|----------------------------------------|
| `version` | The project version that was deployed. |
