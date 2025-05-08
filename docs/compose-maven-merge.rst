.. _rrw-docs-compose-maven-merge:

###################
Compose Maven Merge
###################

This workflow will run when a merge completes for a Java project
built with Maven. It will build the package for release, run tests for final
integration testing, and can optionally sign and push the package for release.

This workflow is directly called, or can serve as a reference workflow for
modification (e.g. removing boolean inputs for pushes that will always occur
or hardcoding inputs/variables that apply to all repos in an organization).

:Required parameters:

    :GERRIT_BRANCH: This parameter comes from the Gerrit trigger.

:Optional parameters:

    :ARTIFACT_DIR: The location of build artifacts. Default:
        "${GITHUB_WORKSPACE}/m2repo"
    :JDK_VERSION: Version of OpenJDK to use. Default: 17
    :MVN_VERSION: Version of Maven to use. Default: 3.8.2
    :MVN_PARAMS: Parameters to pass to the mvn command.
    :MVN_PHASES: List of phases for mvn to execute. Default: "clean deploy"
    :MVN_OPTS: Options to pass to mvn. Default:
        -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=warn
        -Dmaven.repo.local=/tmp/r -Dorg.ops4j.pax.url.mvn.localRepository=/tmp/r
        -DaltDeploymentRepository=staging::default::file:"${GITHUB_WORKSPACE}"/m2repo
    :MVN_POM_FILE: Path to the pom.xml to use. Default: "pom.xml"
    :MVN_PROFILES: Comma-delimited list of profiles to activate.
    :SIGUL_SIGN: Boolean indicating whether to sign artifacts. Default: false
    :PUSH_NEXUS: Boolean indicating whether to push to a Nexus server. Default: false
    :PUSH_ARTIFACTORY: Boolean indicating whether to push to Artifactory. Default: false
    :PUSH_CENTRAL: Boolean indicating whether to push to Maven Central. Default: false
    :inputs.JFROG_OPTIONS: Options passed to the JFrog command when
        pushing to Artifactory (e.g. build number, module).

:Expected variables:

    :vars.GLOBAL_SETTINGS: Global settings.xml used by Maven.

:Expected variables - Sigul signing:

    :secrets.SIGUL_KEY: If SIGUL_SIGN is true, the name of the signing key to
        use (the project's identifier on the Sigul server).
    :secrets.GHA_TOKEN: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_IP: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_URI: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_CONF: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_PASS: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.
    :secrets.SIGUL_PKI: See `sigul-sign-action docs
        <https://github.com/lfit/sigul-sign-action>`_.

:Expected variables - Nexus upload:

    :env.NEXUS_REPO: Environment variable that defines what repo to push to
        (e.g. snapshosts, releases, staging, etc.).
    :vars.NEXUS_SERVER: Nexus server to upload to.
    :secrets.NEXUS_USERNAME: Username for push to Nexus.
    :secrets.NEXUS_PASSWORD: Password or token for push to Nexus.

:Expected variables - Artifactory upload:

    :vars.ARTIFACTORY_URL: Repo URL to push to.
    :secrets.ARTIFACTORY_ACCESS_TOKEN: The token to use to push to Artifactory.
    :vars.ARTIFACTORY_PATH: Path to the repo on Artifactory.

:Expected variables - Central upload:

    :env.NEXUS_REPO: Environment variable that defines what repo to push to
        (e.g. snapshosts, releases, staging, etc.).
    :vars.CENTRAL_URL: Repo URL to push to.
    :secrets.CENTRAL_USERNAME: Username for push to Maven Central.
    :secrets.CENTRAL_PASSWORD: Password or token for push to Maven Central.

..  # SPDX-License-Identifier: Apache-2.0
    # SPDX-FileCopyrightText: Copyright 2025 The Linux Foundation
