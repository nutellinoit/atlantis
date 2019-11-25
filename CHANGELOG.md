# v0.10.1

## Description
Small release that is built using Go 1.13.3 to mitigate a
CVE (https://99designs.ca/blog/engineering/request-smuggling/).

## Features
* Error out when user has an atlantis.yml file (wrong extension, needs .yaml) ([#816](https://github.com/runatlantis/atlantis/pull/816) by @mdcurran)

## Bugfixes
None

## Backwards Incompatibilities / Notes:
If you had an `atlantis.yml` file (note the `.yml` extension), previously Atlantis ignored it.
Now it will error to warn you that it's not being used.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.1/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.10.1`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.10.0..v0.10.1
https://github.com/runatlantis/atlantis/compare/v0.10.0...v0.10.1

# v0.10.0

## Description
Lots of new features in this release: Azure DevOps support,
automatic Terraform version detection and private module cloning support.
All by community contributors!

## Features
* Support for Azure DevOps ([719](https://github.com/runatlantis/atlantis/pull/719) by @mcdafydd)
* Support detecting Terraform version from `terraform { required_version = "=<version>" }` block ([#789](https://github.com/runatlantis/atlantis/pull/789) by @kennethtxytqw)
* Improve `--write-git-creds` command so that it supports ssh private modules ([#799](https://github.com/runatlantis/atlantis/pull/799) by @ImperialXT)
* Default TF version is now 0.12.12
* Logo is now bigger on locks listing ([#783](https://github.com/runatlantis/atlantis/pull/783) by @Nuru)

## Bugfixes
* Fix error when using GitLab with the "Delete source branch" setting (Fixes [#760](https://github.com/runatlantis/atlantis/issues/760))
* Fix repo whitelist when using wildcard in the middle, ex. `github.com/*-something` (Fixes [#692](https://github.com/runatlantis/atlantis/issues/692) by @dedamico)

## Backwards Incompatibilities / Notes:
* If you're using the Atlantis Docker image and aren't setting the `--default-tf-version` flag
  then the default version of Terraform will now be 0.12.12. Simply set the above
  flag to your desired default version to avoid any issues.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.10.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.10.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.9.0..v0.10.0
https://github.com/runatlantis/atlantis/compare/v0.9.0...v0.10.0

# v0.9.0

## Description
This release contains a new step for custom workflows called `env`. It allows
users to set environment variables statically and dynamically for their workflows:
```yaml
workflows:
  env:
    plan:
      steps:
      - env:
          name: STATIC
          value: set-statically
      - env:
          name: DYNAMIC
          command: echo set-dynamically
      - run: echo $STATIC $DYNAMIC # outputs 'set-statically set-dynamically'
```

## Features
* New `env` step in custom workflows ([#751](https://github.com/runatlantis/atlantis/pull/751))
* New flag `--write-git-creds` helps Atlantis support private module sources. ([#711](https://github.com/runatlantis/atlantis/pull/711))
* Upgrade Terraform to 0.12.7 in our base Docker image.
* Support for Terragrunt > 0.19.0 ([#748](https://github.com/runatlantis/atlantis/pull/748))
* The directory where Atlantis downloads Terraform binaries is now in the PATH
of custom workflows ([#678](https://github.com/runatlantis/atlantis/pull/678)) 
* `dumb-init` and `gosu` upgraded in our Docker image ([#730](https://github.com/runatlantis/atlantis/pull/730))

## Bugfixes
* The Terraform version specified in `terraform_version` is now downloaded even
if there are only custom steps (Fixes [#675](https://github.com/runatlantis/atlantis/issues/675))

## Backwards Incompatibilities / Notes:
* If you're using the Atlantis Docker image and aren't setting the `--default-tf-version` flag
  then the default version of Terraform will now be 0.12.7. Simply set the above
  flag to your desired default version to avoid any issues.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.9.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.9.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.9.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.9.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.9.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.8.3..v0.9.0
https://github.com/runatlantis/atlantis/compare/v0.8.3...v0.9.0

# v0.8.3

## Description
This release contains an important security fix in addition to some fixes and
changes for Terraform Cloud/Enterprise users. It's highly recommended that all
Atlantis users upgrade to this release. See the Security
section below for more details.

## Security
* Additional arguments specified in Atlantis comments, ex. `atlantis plan -- -var=foo=bar`
  are now escaped before being appended to the relevant Terraform command. (Fixes [#697](https://github.com/runatlantis/atlantis/pull/697)).
  Previously, a comment like `atlantis plan -- -var=$(touch foo)` would execute
  the `touch foo` command because the extra arguments weren't being escaped properly.
  This means anyone with comment access to an Atlantis repo could execute arbitrary
  code. Because of the severity of this issue, all users should upgrade to this version.
* Upgrade to latest version of Alpine Linux in our Docker image to mitigate
  vulnerabilities found in libssh2. (Fixes [#687](https://github.com/runatlantis/atlantis/issues/687))

## Features
* Upgrade Terraform to 0.12.3 in our base Docker image.
* Additional arguments specified in Atlantis comments, ex. `atlantis plan -- -var=foo=bar`
  are now available in custom run steps as the `COMMENT_ARGS` environment variable. (Fixes [#670](https://github.com/runatlantis/atlantis/issues/670))
* A new flag `--tfe-hostname` is available for specifying a Terraform Enterprise private installation's hostname
  when using the remote backend integration. ([#706](https://github.com/runatlantis/atlantis/pull/706))

## Bugfixes
* Parse Bitbucket Cloud pull request rejected events properly. (Fixes [#676](https://github.com/runatlantis/atlantis/issues/676))
* Terraform >= 0.12.0 works with Terraform Cloud/Enterprise remote operations. (Fixes [#704](https://github.com/runatlantis/atlantis/issues/704))

## Backwards Incompatibilities / Notes:
* If you were previously relying on being able to execute code in the additional
  arguments of comments, ex. `atlantis plan -- -var='foo=$(echo $SECRET)'` this
  is no longer possible. Instead you will need to write a custom workflow with a
  custom step or the extra_args config.
* If you're using the Atlantis Docker image and aren't setting the `--default-tf-version` flag
  then the default version of Terraform will now be 0.12.3. Simply set the above
  flag to your desired default version to avoid any issues.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.3/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.3/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.3/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.3/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.8.3`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.8.2..v0.8.3
https://github.com/runatlantis/atlantis/compare/v0.8.2...v0.8.3

# v0.8.2

## Description
Small bugfix release for Bitbucket Cloud users running with "require mergeable".

## Features
* Update default Terraform version to 0.12.1.
* Include directory in Slack message ([#660](https://github.com/runatlantis/atlantis/issues/660)).

## Bugfixes
* Atlantis would not allow applies for all Bitbucket Cloud pull requests if running with "require mergeable" 
  even if the pull request *was* mergeable due to an API change. (Fixes [#672](https://github.com/runatlantis/atlantis/issues/672))

## Backwards Incompatibilities / Notes:
* If you're using the Atlantis Docker image and aren't setting the `--default-tf-version` flag
  then the default version of Terraform will now be 0.12.1. Simply set the above
  flag to your desired default version to avoid any issues.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.2/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.2/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.2/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.2/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.8.2`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.8.1..v0.8.2
https://github.com/runatlantis/atlantis/compare/v0.8.1...v0.8.2

# v0.8.1

## Description
Small bugfix release for Bitbucket Cloud users running with require approval.

## Features
None

## Bugfixes
* Atlantis would panic when checking if pull requests were approved for Bitbucket
  Cloud due to an API change. (Fixes [#652](https://github.com/runatlantis/atlantis/issues/652))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.1/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.8.1`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.8.0..v0.8.1
https://github.com/runatlantis/atlantis/compare/v0.8.0...v0.8.1

# v0.8.0

## Description
This release upgrades the default version of Terraform to 0.12.
If you're running Atlantis with the `--default-tf-version` flag set (which
you always should) then this won't affect you at all.

## Features
* Upgrade default Terraform version to 0.12
* Add new `--disable-apply-all` flag that disables running `atlantis apply`
  without any flags. ([#645](https://github.com/runatlantis/atlantis/pull/645))

## Bugfixes
None

## Backwards Incompatibilities / Notes:
* If you're using the Atlantis Docker image and aren't setting the `--default-tf-version` flag
  then the default version of Terraform will now be 0.12. Simply set the above
  flag to your desired default version of Terraform and 0.12 won't be used.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.8.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.8.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.7.2..v0.8.0
https://github.com/runatlantis/atlantis/compare/v0.7.2...v0.8.0

# v0.7.2

## Description
Small release containing an important security fix and some bugfixes.

## Features
None

## Bugfixes
* Atlantis would post its Git credentials as pull request comment and in logs if the git clone failed. (Fixes [#615](https://github.com/runatlantis/atlantis/issues/615))
* Atlantis would comment the same output twice during errors of custom run steps. (Fixes [#519](https://github.com/runatlantis/atlantis/issues/519))
* `atlantis testdrive` had unreadable output on solarized terminals. (Fixes [#575](https://github.com/runatlantis/atlantis/issues/575))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.2/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.2/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.2/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.2/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.7.2`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.7.1..v0.7.2
https://github.com/runatlantis/atlantis/compare/v0.7.1...v0.7.2

# v0.7.1

## Description
Small bugfix release to fix an issue when using `--checkout-strategy=merge`.

## Features
* `PROJECT_NAME` is now available as an environment variable to custom `run` steps. ([#578](https://github.com/runatlantis/atlantis/pull/578))

## Bugfixes
* Fix deleting unapplied plans when `--checkout-strategy=merge` is used. (Fixes [#582](https://github.com/runatlantis/atlantis/issues/582))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.1/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.7.1`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.7.0..v0.7.1
https://github.com/runatlantis/atlantis/compare/v0.7.0...v0.7.1

# v0.7.0

## Description
This release implements Server-Side Repo Config which allows users to write
`atlantis.yaml`-style config on the server rather than in individual repos.
The Server Side config also allow Atlantis operators to control what individual
repos can do in their `atlantis.yaml` files. Read [docs](https://www.runatlantis.io/docs/server-side-repo-config.html) for more details.

## Features
* Server-Side Repo Config. Read [docs](https://www.runatlantis.io/docs/server-side-repo-config.html)
  and [use cases](https://www.runatlantis.io/docs/server-side-repo-config.html#use-cases) for full details. ([#47](https://github.com/runatlantis/atlantis/issues/47))
  * New flag `atlantis server` flag `--repo-config` for specifying the
    repo config file .
  * New flag `--repo-config-json` for specifying the repo config as a JSON string
    instead of having to write a config file to disk.
  * All repos can now create `atlantis.yaml` files to configure their projects,
    however by default, those files can't create custom workflows or set Apply
    Requirements.
* New version `3` of `atlantis.yaml` fixes a small issue with how we were parsing
  custom `run` steps. Previously we were doing additional parsing which caused some
  users to have to add extra escaping to their commands. Now this is no longer
  required. See the Backwards Compatibility section for more details.

## Bugfixes
* Fix bug where running `atlantis apply` to apply all outstanding plans wouldn't work if
  you had more than one project defined in the exact same directory and workspace. (Fixes [#365](https://github.com/runatlantis/atlantis/issues/365))

## Backwards Incompatibilities / Notes:
* The server-side config changes are fully backwards compatible. The biggest
  difference is that all repos can now create `atlantis.yaml` files, but without
  being able to create custom workflows or set apply requirements. This will
  allow users to configure their projects, workspaces and terraform versions
  at a repo level without enabling those repos to run custom code or circumvent
  apply requirements set server-side.
* `atlantis.yaml` has a new version `3`. If you continue to use version `2`, you
  will experience no changes. If you want to upgrade to version `3`, then
  if you're not using any custom `run` steps in your workflows you can upgrade
  the version number without additional changes.
  
  If you are using `run` steps, check our [upgrade guide](https://www.runatlantis.io/docs/upgrading-atlantis-yaml.html#upgrading-from-v2-to-v3)
  to see if you need to make any changes before upgrading.
* Flags `--require-approval`, `--require-mergeable` and `--allow-repo-config` are
  deprecated in favour of creating a server-side repo config file that applies
  the same configuration. If you run `atlantis server` with those flags, a
  deprecation warning will be printed telling you what server-side config is
  recommended instead.
* If you have projects configured with the same directory and workspace (which means
  you're probably using the `-backend-config` flag) **and** their names contain `/`'s,
  then you'll have to re-run `atlantis plan` after upgrading if you had any unapplied plans.
  
  An example of what config would mean you need to re-plan:
  ```yaml
  projects:
  - name: name/with/slashes
    dir: samedir
    workflow: a
  - name: another/with/slashes
    dir: samedir
    workflow: b
  a:
     plan:
       steps:
       - run: rm -rf .terraform
       - init:
           extra_args: [-backend-config=staging.backend.tfvars]
       - plan
  b:
     plan:
       steps:
       - run: rm -rf .terraform
       - init:
           extra_args: [-backend-config=staging.backend.tfvars]
       - plan
  ```

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.7.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.7.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.6.0..v0.7.0
https://github.com/runatlantis/atlantis/compare/v0.6.0...v0.7.0

# v0.6.0

## Description
This release introduces a new flag `--default-tf-version=<version>` that allows users
to set the version of Terraform that Atlantis defaults to. Atlantis will automatically
download that version on startup so users don't need to build their own custom
Docker images.

Atlantis will also now automatically download any Terraform version specified in
`atlantis.yaml`:
```yaml
version: 2
projects:
- dir: .
  terraform_version: v0.12.0-beta1 # Will be downloaded automatically.
```

## Features
* New flag: `--default-tf-version=<version>` will cause Atlantis to automatically download
  and use that version of Terraform by default. Atlantis will also automatically
  download terraform versions specified in `atlantis.yaml` via the `terraform_version`
  config key. ([#538](https://github.com/runatlantis/atlantis/pull/538))
* New status check names mean that the Atlantis checks will appear together (at least on GitHub).
  ([#545](https://github.com/runatlantis/atlantis/pull/545))
* Upgrade base Docker image to use Alpine 3.9. Alpine 3.9 mitigates
  [CVE-2018-19486](https://nvd.nist.gov/vuln/detail/CVE-2018-19486). ([#541](https://github.com/runatlantis/atlantis/pull/541))

## Bugfixes
None

## Backwards Incompatibilities / Notes:
* Our Docker image `runatlantis/atlantis` has Terraform `v0.11.13` now. If you
  use the new flag `--default-tf-version=<desired version>` then you won't
  be affected by this change (nor for subsequent version upgrades).
* The Atlantis status checks have been renamed from what they looked like in `v0.5.*`.
  Previously the names were: `plan/atlantis` and `apply/atlantis`. Now the
  names are `atlantis/plan` and `atlantis/apply`.
  
  This change will only affect you if you're requiring those status checks to pass via a setting in
  your Git host (ex. via GitHub protected branches). If so, you'll need to change
  your settings to require the new names to pass and un-require the old names.
  
  > If you were on a version lower than `v0.5.*` then read the backwards compatiblity
    notes for release `0.5.0`.
    
  **NOTE from the maintainer**: I take backwards compatibility seriously and I
  apologize that the status checks are changing again so soon after the 0.5 release
  also changed them. I know that if you have many repos and require the checks
  to pass that it is a large task to change them all again.
  
  In this case, I decided that the tradeoff was worth it because the
  0.5 release has only been out for a couple of weeks so hopefully not everyone
  has upgraded to it. The new check names makes them a lot easier to read
  (at least on GitHub) because they appear next to each other now due to
  alphabetical sorting. In this case I felt like it was better to get this change
  done as soon as possible rather than having this annoying UX issue stay around
  forever.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.6.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.6.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.6.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.6.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.6.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

## Diff v0.5.1..v0.6.0
https://github.com/runatlantis/atlantis/compare/v0.5.1...v0.6.0

# v0.5.1

## Description
This is a bugfix release to fix a bug where Atlantis was replying to comments
that weren't directed to it.

Diff: https://github.com/runatlantis/atlantis/compare/v0.5.0...v0.5.1

## Features
* On Bitbucket Cloud and Server, Atlantis now responds if it's invoked with the
  username it's running under, ex. @my-bb-atlantis-user. This is the same
  functionality as GitHub and GitLab. ([#534](https://github.com/runatlantis/atlantis/pull/534))

## Bugfixes
* Atlantis ignore comments that aren't addressed to it. (Fixes [#533](https://github.com/runatlantis/atlantis/issues/533))

## Backwards Incompatibilities / Notes:
* On Bitbucket Cloud and Server, Atlantis now responds if it's invoked with the
  username it's running under, ex. @my-bb-atlantis-user. This is the same
  functionality as GitHub and GitLab.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.1/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.5.1`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.5.0

## Description
This release has two big features: New Status Checks and Terraform Enterprise
Integration.

**New Status Checks:**

The new status checks split the old status check into `plan` and `apply` phases.
Each check now tracks the status of each project modified in the pull request.
For example if two projects are modified, the `plan` check might read:
> 2/2 projects planned successfully.

And the `apply` check might read:
> 0/2 projects applied successfully.

Users can now use their Git host's settings to require these checks pass
before a pull request is merged and be confident that all changes have been
applied (for example).

**Terraform Enterprise Integration:**

Atlantis now integrates with the Terraform Enterprise (TFE)
via the [remote backend](https://www.terraform.io/docs/backends/types/remote.html).
Atlantis will run `terraform` commands as usual, however those commands will
actually be executed *remotely* in Terraform Enterprise.

Using Atlantis with Terraform Enterprise gives you access to TFE features like:
* Real-time streaming output
* Ability to cancel in-progress commands
* Secret variables
* [Sentinel](https://www.hashicorp.com/sentinel)
Without having to change your pull request workflow.

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.15...v0.5.0

## Features
* Split single status check into one for `plan` and one for `apply` (see above).
* Support using Atlantis with Terraform Enterprise via
 [remote operations](https://www.terraform.io/docs/backends/operations.html) (see above).
* Add `USER_NAME` environment variable for custom steps to use. ([#489](https://github.com/runatlantis/atlantis/pull/489))
* Support Bitbucket Cloud's upcoming API deprecations. ([#502](https://github.com/runatlantis/atlantis/pull/502))
* Support Bitbucket Server hosted at a basepath, ex. `bitbucket.mycompany.com/pathprefix` (Fixes [#508](https://github.com/runatlantis/atlantis/issues/508))

## Bugfixes
* Allow Bitbucket Server diagnostics checks. (Fixes [#474](https://github.com/runatlantis/atlantis/issues/474))
* Fix automerge for Bitbucket Server. (Fixes [#479](https://github.com/runatlantis/atlantis/issues/479))
* Run `terraform init` with `-upgrade`. (Fixes [#443](https://github.com/runatlantis/atlantis/issues/443))
* If a pull request is deleted in Bitbucket Server, delete locks. (Fixes [#498](https://github.com/runatlantis/atlantis/issues/498))
* Support directories with spaces, ex `atlantis plan -d 'dir with spaces'`. (Fixes [#423](https://github.com/runatlantis/atlantis/issues/423))
* Ignore Terragrunt cache directories that were causing duplicate applies. (Fixes [#487](https://github.com/runatlantis/atlantis/issues/487))
* Fix `atlantis testdrive` for latest version of ngrok.

## Backwards Incompatibilities / Notes:
* **New Status Checks** - If you have settings in your Git host that require the Atlantis commit status
  check to be in a certain condition, you will need to modify that setting as follows:

  Previously, Atlantis set a single check with the name `Atlantis`. Now there are
  two checks with the names `plan/atlantis` and `apply/atlantis`. If you had
  previously required the `Atlantis` check to pass, you should now require both
  the `plan/atlantis` and `apply/atlantis` checks to pass.

  The behaviour has also changed. Previously, the single Atlantis check
  would represent the status of the **last
  run command**. For example, if I ran `atlantis plan` and it failed, the check
  would be in a *Failed* state. If I ran `atlantis apply -p project1` and it succeeded,
  then the check would be in a *Success* state, regardless of the status of other projects
  in the pull request.

  Now, each check represents the plan/apply status of **all** projects modified in
  the pull request. For example, say I open up a pull request that modifies
  two projects, one in directory `proj1` and the other in `proj2`. If autoplanning
  is enabled, and both plans succeed, then there will be a single status check:
  * `plan/atlantis - 2/2 projects planned successfully` (success)

  If I run `atlantis apply -d proj1`, then Atlantis will set a pending apply check:
  * `plan/atlantis - 2/2 projects planned successfully` (success)
  * `apply/atlantis - 1/2 projects applied successfully` (pending)

  If I apply the final project with `atlantis apply -d proj2`, then my checks
  will look like:
  * `plan/atlantis - 2/2 projects planned successfully` (success)
  * `apply/atlantis - 2/2 projects applied successfully` (success)

* `terraform init` is now run with `-upgrade=true`. Previously, it used Terraform's
  default setting which was `false`.

  This means that `terraform` will always update to the latest version of plugins
  and modules. For example, if you're using a module source of
    ```hcl
    source = "git::https://example.com/vpc.git?ref=master"
    ```
  then `terraform init` will now always use the version on `master` whereas
  previously, if you had already run `atlantis plan` before `master` was updated,
  a new `atlantis plan` wouldn't pull the latest changes and would just use
  the cached version.
  
  This is unlikely to cause any issues because most users already expected Atlantis
  to use the most up-to-date version of modules/plugins within the set constraints.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.5.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.5.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.15

## Description
This is a bugfix release containing an important fix to how Atlantis executes Terraform. A
bug was introduced in v0.4.14 that causes Atlantis to hang indefinitely when
executing Terraform when there is a lot of output from Terraform.

In addition, there's a fix to automerge when you require rebasing or commit
squashing in GitHub and a fix for the mergeability check if you're requiring
the Atlantis status to pass in GitHub.

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.14...v0.4.15

## Features
None – this is a bugfix release.

## Bugfixes
* Atlantis hangs on large plans. (Fixes [#474](https://github.com/runatlantis/atlantis/issues/474))
* Automerge now works on GitHub if you require a rebase or squash merge. ([#466](https://github.com/runatlantis/atlantis/pull/466))
* Automerge now works on Bitbucket if previously you were getting XSRF errors. (Fixes [#465](https://github.com/runatlantis/atlantis/issues/465))
* Requiring `mergeable` now works on GitHub if you are also requiring the Atlantis status to pass before merging. (Fixes [#453](https://github.com/runatlantis/atlantis/issues/453))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.15/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.15/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.15/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.15/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.15`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.14

## Description
**WARNING:** This release contains a bug that causes Terraform execution to stall
on large infrastructures. Please use v0.4.15 instead.

This release contains two big new features: Automerge and Checkout Strategy.

Automerge is a much asked for feature that allows Atlantis to automatically
merge your pull requests if all plans have been applied successfully.
It can be enabled via the `--automerge` flag, or via an `atlantis.yaml` setting:
```yaml
version: 2
automerge: true
projects:
- ...
```

Checkout Strategy allows you to choose if Atlantis checks out the exact branch
from the pull request or what the destination branch will look like once the pull
request is merged. You can choose your checkout strategy via the `--checkout-strategy`
flag which supports `branch` (the default) or `merge`.

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.13...v0.4.14

## Features
* Can now be configured to **automatically merge pull requests** after all plans have
  been applied. See https://www.runatlantis.io/docs/automerging.html. (Fixes [#186](https://github.com/runatlantis/atlantis/issues/186))
* New `--checkout-strategy` flag which supports checking out the code as it will
  look once the pull request was merged. Previously we only supported checking out
  the pull request branch which might be out of date with the destination branch
  and so cause Terraform to delete resources that have already been applied.
  See https://www.runatlantis.io/docs/checkout-strategy.html. (Fixes [#35](https://github.com/runatlantis/atlantis/issues/35)
* Support Terraform 0.12 by version detection and then changing how Atlantis runs
  its Terraform commands. ([#419](https://github.com/runatlantis/atlantis/pull/419))
* New `--tfe-token` flag to support using Terraform Enterprise's Free Remote State Storage. ([#419](https://github.com/runatlantis/atlantis/pull/419))

## Bugfixes
* Run plan in directory when file is moved. (Fixes [#413](https://github.com/runatlantis/atlantis/issues/413))
* Fix bug where when Terraform crashed, Atlantis would hang indefinitely. ([#421](https://github.com/runatlantis/atlantis/pull/421))

## Backwards Incompatibilities / Notes:
None

## Downloads
**The release downloads have been deleted because this release contains a critical bug**

## Docker
**The release downloads have been deleted because this release contains a critical bug**

# v0.4.13

## Description
This release is focused on quick-wins, bugfixes and one new feature that allows
users to require pull requests be "mergeable", before allowing for `atlantis apply`.

The mergeable apply requirement is very useful for GitHub users where it allows
them to require pull requests be approved by specific users or require certain
status checks to pass. See https://www.runatlantis.io/docs/apply-requirements.html#mergeable for
more information.

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.12...v0.4.13

## Features
* Introduce a new (optional) `mergeable` apply requirement that requires pull requests to be mergeable prior to allowing `apply` to run. (Fixes [#43](https://github.com/runatlantis/atlantis/issues/43))
* If users have workspaces configured for a directory via an `atlantis.yaml` file, only allow
    commands to be run on those workspaces. All commands attempted to be run on different workspaces will error out.

    For example, if I have an `atlantis.yaml` file:
    ```yaml
    version: 2
    projects:
    - dir: mydir
      workspace: default
    - dir: mydir
      workspace: staging
    ```
    Then I can run `atlantis apply -d mydir -w default` and `atlantis apply -d mydir -w staging`
    but I will receive an error if I run `atlantis apply -d mydir -w somethingelse`.

* If users are setting the `name` key for their projects in `atlantis.yaml`, then
  include the project name in the comment output so it's easier to identify which
  plan/apply output is for which project. (Fixes [#353](https://github.com/runatlantis/atlantis/issues/353)))
* Bump the Terraform version in the Docker image to `0.11.11`.
* Tweak logging to add timezone to the timestamp and make the output more readable. ([#402](https://github.com/runatlantis/atlantis/pull/402))
* Warn users if running `atlantis apply -- -target=myresource` because `-target` can
  only be specified during `atlantis plan`. (Fixes [#399](https://github.com/runatlantis/atlantis/issues/399))

## Bugfixes
* If `terraform plan` returns an error, print the error to the pull request. ([#381](https://github.com/runatlantis/atlantis/pull/381))
* Split Bitbucket Server comments into multiple comments if over the max size. (Fixes [#280](https://github.com/runatlantis/atlantis/issues/280))
* Fix issue where if users specified `--gitlab-hostname` without a scheme then Atlantis wouldn't parse the URL correctly. ([#377](https://github.com/runatlantis/atlantis/issues/377))
* Give better error message if GitLab users are commenting on commits instead of a merge request. (Fixes [#150](https://github.com/runatlantis/atlantis/issues/150), [#390](https://github.com/runatlantis/atlantis/issues/390))
* If an error occurs early in request processing, comment that error back on the pull request.
  Previously, we *were* commenting back on errors but not for errors very early in the processing. (Fixes [#398](https://github.com/runatlantis/atlantis/issues/398))

## Backwards Incompatibilities / Notes:
* The version of Terraform installed in the `runatlantis/atlantis` Docker image
  is now `0.11.11`. Previously it was `0.11.10`.
* If you are a) using an `atlantis.yaml` file and b) defining Terraform workspaces
    and c) running plan and apply against workspaces that **were not** defined in the
    `atlantis.yaml` file, then this no longer works.

    You will now need to define all the workspaces in the `atlantis.yaml` file.
    For example, say you had the following config:
    ```yaml
    version: 2
    projects:
    - dir: mydir
      workspace: production
    ```

    And you used to run:
    ```
    atlantis plan -d mydir -w anotherworkspace
    atlantis apply -d mydir -w anotherworkspace
    ```

    For this to work now, you need to add the `anotherworkspace` workspace to your
    `atlantis.yaml` file:
    ```yaml
    version: 2
    projects:
    - dir: mydir
      workspace: production
    - dir: mydir
      workspace: anotherworkspace
    ```


## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.13/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.13/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.13/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.13/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.13`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.12

## Description
Small feature and bug fix release. If you're using GitLab <11.1 then your
comment formatting is fixed!

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.11...v0.4.12

## Features
- Atlantis can now be hosted behind a path-based router and its UI will still
  render correctly. For example, you could host atlantis at mydomain.com/mypath,
  then run `atlantis server --atlantis-url https://mydomain.com/mypath` and when
  atlantis renders its UI, all the URLs will have the `/mypath` prefix so the UI
  renders properly. (Fixes [#213](https://github.com/runatlantis/atlantis/issues/213))
- Log warning if GitLab hostname isn't resolvable. (Fixes [#359](https://github.com/runatlantis/atlantis/issues/359))
- Support running our official Docker image `runatlantis/atlantis` on OpenShift. OpenShift runs images
  with random uids so we needed to build in support for that. (Fixes [#345](https://github.com/runatlantis/atlantis/issues/345))

## Bugfixes
- If the output is too long for a single GitHub comment, maintain formatting when
  splitting into multiple comments. (Fixes [#111](https://github.com/runatlantis/atlantis/issues/111))
- Fix bug with using the pagination API in BitBucket. ([#354](https://github.com/runatlantis/atlantis/pull/354))
- If using GitLab < 11.1 then don't use expandable markdown comments. (Fixes [#315](https://github.com/runatlantis/atlantis/issues/315))
- Fix output from custom steps that came before the plan step from being removed. ([#367](https://github.com/runatlantis/atlantis/pull/367))

## Backwards Incompatibilities / Notes:
We made [changes](https://github.com/runatlantis/atlantis/pull/346) to the base image (`runatlantis/atlantis-base`) that
`runatlantis/atlantis` is built off of. These changes **should not** affect your
running of atlantis unless you're building your own custom images and were relying
on specific user permissions. Even then we don't anticipate any problems.

These are the changes in detail:
1. Previously, the permissions of `/home/atlantis` were:
     ```bash
     $ ls -la /home/atlantis/
     drwxr-sr-x    2 atlantis atlantis      4096 Sep 13 22:49 .
     ```
   Now they are:
    ```bash
    $ ls -la /home/atlantis/
    drwxrwxr-x    2 atlantis root          4096 Nov 28 21:22 .
    ```
   * The directory is now owned by the `root` group.
   * Its group permissions now include `w` and `x`.

   This was needed because OpenShift runs Docker images as random uid's under
   the root group and so now those random uid's can use `/home/atlantis` as their
   data directory.

1. Previously, the `atlantis` user was only part of its own group:
    ```bash
    $ gosu atlantis sh
    $ whoami
    atlantis
    $ groups
    atlantis
    ```

    Now it's also part of the `root` group:
    ```bash
    $ gosu atlantis sh
    $ groups
    atlantis root
    ```
1. Previously, the permissions for `/etc/passwd` were:
    ```bash
    $ ls -la /etc/passwd
    -rw-r--r--    1 root     root          1284 Sep 13 22:49 /etc/passwd
    ```

    Now the permissions are:
    ```bash
    $ ls -la /etc/passwd
    -rw-rw-r--    1 root     root          1284 Nov 28 21:22 /etc/passwd
    ```

    The `w` group permission was added so that in OpenShift, the random uid can write
    their own login entry (https://github.com/runatlantis/atlantis/blob/master/docker-entrypoint.sh#L28)
    which is required because `terraform` expects the running user to have an entry
    in `/etc/passwd`.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.12/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.12/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.12/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.12/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.12`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.11

## Description
Medium sized release that updates the Terraform version and makes `terraform plan`
output smaller by removing the `Refreshing...` output.

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.10...v0.4.11

## Features
* Upgraded Docker image to use Terraform 0.11.10
* `terraform plan` output is shorter now thanks to remove the `Refreshing...` output ([#339](https://github.com/runatlantis/atlantis/pull/339))
* Project names specified in `atlantis.yaml` can now contain `/`'s. This is useful
if you want to name your projects similar to the directories they're in. (Fixes [#253](https://github.com/runatlantis/atlantis/issues/253))
* Added new flag `--silence-whitelist-errors` which prevents Atlantis from comment back on pull requests
from non-whitelisted repos. This is useful if you want to add the Atlantis webhook to a whole organization
and then control which repos are actioned on via the whitelist. (Fixes [#312](https://github.com/runatlantis/atlantis/issues/312))
* The message when the project is locked is now more helpful. ([#336](https://github.com/runatlantis/atlantis/pull/336))
* Run `terraform plan` with `-var atlantis_repo_owner=runatlantis -var atlantis_repo_name=atlantis -var atlantis_pull_num=10`
(if the repo was runatlantis/atlantis) ([#300](https://github.com/runatlantis/atlantis/pull/300))

## Bugfixes
* Quote plan filenames so that Bitbucket projects with spaces in their names still work (Fixes [#302](https://github.com/runatlantis/atlantis/issues/302))

## Backwards Incompatibilities / Notes:
* Atlantis now runs `terraform plan` with
    ```bash
    -var atlantis_repo_owner=runatlantis \
    -var atlantis_repo_name=atlantis \
    -var atlantis_pull_num=10
    ```

    (in this example the repo that Atlantis is running on is runatlantis/atlantis).

    If you were using those variables in your terraform code:
    ```hcl
    variable "atlantis_repo_owner" {
      default = "my_default"
    }
    ```

    Then Atlantis will be overriding those variables with its own values. To prevent
    this, you need to rename your variables.

    If you aren't using those variables then this change won't affect you.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.11/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.11/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.11/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.11/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.11`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.10

## Description
Small bugfix release to fix issues with new comment format.

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.9...v0.4.10

## Features
None

## Bugfixes
* Fix bad comment rendering ([#294](https://github.com/runatlantis/atlantis/issues/294))
* Fix `plan` not working on Bitbucket Server when repo owner contains spaces ([#290](https://github.com/runatlantis/atlantis/issues/290))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.10/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.10/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.10/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.10/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.10`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.9

## Description
This release is mostly focused on changing how comments look. Terraform output
is now automatically hidden if it's over 12 lines long:
![https://user-images.githubusercontent.com/1034429/45580771-d4603b80-b849-11e8-8c4b-5984bd0bff7f.png](https://user-images.githubusercontent.com/1034429/45580771-d4603b80-b849-11e8-8c4b-5984bd0bff7f.png)
Also the red and green highlighting for added and removed resources is fixed:
![https://user-images.githubusercontent.com/1034429/45580777-d9bd8600-b849-11e8-8f2d-867fbf4e72d7.png](https://user-images.githubusercontent.com/1034429/45580777-d9bd8600-b849-11e8-8f2d-867fbf4e72d7.png)

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.8...v0.4.9

## Features
* Terraform output over 12 lines is hidden in comment until expanded
* `terraform plan` output is highlighted correctly
* Terraform is now executed with `-var atlantis_repo={repo name} -var atlantis_pull_num {pull num}`.
This will allow users to trace Atlantis `terraform` executions in CloudTrail back to a specific
user and pull request if using assume role by creating a specific name for the session Terraform initiates.
```
provider "aws" {
  assume_role {
    role_arn     = "arn:aws:iam::ACCOUNT_ID:role/ROLE_NAME"
    session_name = "${var.atlantis_user}-${var.atlantis_repo}-${var.atlantis_pull_num}"
  }
}
```

## Bugfixes
* Run terraform with `-input=false` ([#268](https://github.com/runatlantis/atlantis/issues/268)).

## Backwards Incompatibilities / Notes:
* We set two new Terraform variables: `atlantis_repo` and `atlantis_pull_num`. If
you were using variables with those names in your code you will need to rename them
in your code.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.9/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.9/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.9/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.9/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.9`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.8

## Description
Security release to upgrade the Docker image to the latest version of Alpine linux that fixes
this bug: https://justi.cz/security/2018/09/13/alpine-apk-rce.html

Diff: https://github.com/runatlantis/atlantis/compare/v0.4.7...v0.4.8

## Features
None

## Bugfixes
* Change server startup message to INFO from WARN level.

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.8/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.8/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.8/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.8/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.8`](https://hub.docker.com/r/runatlantis/atlantis/tags/)


# v0.4.7

## Description
Support GitLab repos nested under multiple levels and use the latest version of Terraform: 0.11.8!

## Features
* Support GitLab groups which allow repos to be nested under multiple levels,
ex. `gitlab.com/owner/group/subgroup/subsubgroup/repo`
* Use latest version of Terraform: 0.11.8 in Docker image

## Bugfixes
* When running with `TF_LOG` set, Atlantis will start normally. Previously it
would error out due to attempting to parse the stderr output of the `terraform version`
command.

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.7/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.7/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.7/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.7/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.7`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.6

## Description
Just a small bugfix release.

## Features
None

## Bugfixes
* If `terraform init` fails, include the failure logs in the comment posted back to the PR.

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.6/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.6/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.6/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.6/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.6`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.5

## Features
* `atlantis apply` now applies **all** unapplied plans instead of just the plan in the root directory. ([#169](https://github.com/runatlantis/atlantis/issues/169))
* `atlantis plan` now plans **all** modified projects instead of just the root directory.
* Plan comments now contain instructions for how to run apply or re-run plan.

## Bugfixes
* Ignore approvals from the pull request author (Bitbucket Cloud only). Fixes ([#201](https://github.com/runatlantis/atlantis/issues/201))
* When double clicking on a GitHub comment, ex.
    ```
    atlantis apply
    ```
  GitHub would add two newlines to the end. If this was then pasted into a new
  comment, Atlantis would accept it because of the extra newlines. This has been fixed
  and the comment with two newlines will be accepted.

## Backwards Incompatibilities / Notes:
* `atlantis apply` now applies **all** unapplied plans. Previously it would only apply the plan in the root directory and default workspace.
* `atlantis plan` now plans **all** modified projects. Previously it would only run plan in the root directory and default workspace.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.5/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.5/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.5/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.5/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.5`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.4

## Features
* Supports Bitbucket Server ([#190](https://github.com/runatlantis/atlantis/issues/190)).

## Bugfixes
* Fix `/etc/hosts` not being respected ([#196](https://github.com/runatlantis/atlantis/issues/196)).

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.4/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.4/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.4/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.4/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.4`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.3

## Features
* Supports Bitbucket Cloud (bitbucket.org) ([#30](https://github.com/runatlantis/atlantis/issues/30)).

## Bugfixes
None

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.3/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.3/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.3/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.3/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.3`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.2

## Features
* Don't comment on pull request if autoplan determines there are no projects to plan in.
This was getting very noisy for users who use their repos for more than just Terraform ([#183](https://github.com/runatlantis/atlantis/issues/183)).

## Bugfixes
None

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.2/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.2/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.2/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.2/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.2`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.1

## Features
* Add new `/healthz` endpoint for health checking in Kubernetes ([#102](https://github.com/runatlantis/atlantis/issues/102))
* Set `$PLANFILE` environment variable to expected location of plan file when running custom steps ([#168](https://github.com/runatlantis/atlantis/issues/168))
    * This enables overriding the command Atlantis uses to `plan` and substituting your own or piping through a custom script.
* Changed default pattern to detect changed files to `*.tf*` from `*.tf` in order
to trigger on `.tfvars` files.

## Bugfixes
None

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.1/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.1`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.4.0

## Features
* Autoplanning - Atlantis will automatically run `plan` on new pull requests and
when new commits are pushed to the pull request.
* New repository `atlantis.yaml` format that supports:
    * Complete customization of plans run
    * Single config file for whole repository
    * Controlling autoplanning
* Moved docs to standalone website from the README.
* Fixes:
    * [#113](https://github.com/runatlantis/atlantis/issues/113)
    * [#50](https://github.com/runatlantis/atlantis/issues/50)
    * [#46](https://github.com/runatlantis/atlantis/issues/46)
    * [#39](https://github.com/runatlantis/atlantis/issues/39)
    * [#28](https://github.com/runatlantis/atlantis/issues/28)
    * [#26](https://github.com/runatlantis/atlantis/issues/26)
    * [#4](https://github.com/runatlantis/atlantis/issues/4)

## Bugfixes

## Backwards Incompatibilities / Notes:
- The old `atlantis.yaml` config file format is not supported. You will need to migrate to the new config
format, see: https://www.runatlantis.io/docs/upgrading-atlantis-yaml.html
- To use the new config file, you must run Atlantis with `--allow-repo-config`.
- Atlantis will now try to automatically plan. To disable this, you'll need to create an `atlantis.yaml` file
as follows:
```yaml
version: 2
projects:
- dir: mydir
  autoplan:
    enabled: false
```
- `atlantis apply` no longer applies all un-applied plans but instead applies only the plan in the root directory and default workspace. This will be reverted in an upcoming release
- `atlantis plan` no longer plans in all modified projects but instead runs plan only in the root directory and default workspace. This will be reverted in an upcoming release.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.4.0/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.4.0`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.3.11

## Features
None

## Bugfixes
* If the `TF_LOG` environment variable is set, should still be able to start. Previously `atlantis server` would exit immediately because it couldn't parse the output of `terraform version`.

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.11/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.11/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.11/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.11/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.3.11`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.3.10

## Features
* Rename `atlantis bootstrap` to `atlantis testdrive` to make it clearer that it
doesn't set up Atlantis for you. Fixes ([#129](https://github.com/runatlantis/atlantis/issues/129)).
* Atlantis will now comment on a pull request when a plan/lock is discarded from
the Atlantis UI. Fixes ([#27](https://github.com/runatlantis/atlantis/issues/27)).

## Bugfixes
* Fix issue during `atlantis bootstrap` where ngrok tunnel took a long time to start.
Atlantis will now wait until it sees the expected log entry before continuing.
Fixes ([#92](https://github.com/runatlantis/atlantis/issues/92)).
* Fix missing error checking during `atlantis bootstrap`. ([#130](https://github.com/runatlantis/atlantis/pulls/130)).

## Backwards Incompatibilities / Notes:
* `atlantis bootstrap` renamed to `atlantis testdrive`

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.10/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.10/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.10/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.10/atlantis_linux_arm.zip)

## Docker
[`runatlantis/atlantis:v0.3.10`](https://hub.docker.com/r/runatlantis/atlantis/tags/)

# v0.3.9

## Features
* None

## Bugfixes
* Fix GitLab approvals not actually checking approval ([#114](https://github.com/runatlantis/atlantis/issues/114))

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.9/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.9/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.9/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.9/atlantis_linux_arm.zip)

# v0.3.8

## Features
* Terraform 0.11.7 in Docker image
* Docker build now verifies terraform install via checksum

## Bugfixes
* None

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.8/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.8/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.8/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.8/atlantis_linux_arm.zip)

# v0.3.7

## Bugfixes
* `--repo-whitelist` is now case insensitive. Fixes ([#95](https://github.com/runatlantis/atlantis/issues/95)).

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.7/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.7/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.7/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.7/atlantis_linux_arm.zip)

# v0.3.6

## Features
* `atlantis server -h` has newlines between flags so it's easier to read ([#91](https://github.com/runatlantis/atlantis/issues/91)).

## Bugfixes
* `atlantis bootstrap` uses a custom ngrok config file so it should work even
if the user is already running another ngrok tunnel ([#93](https://github.com/runatlantis/atlantis/issues/93)).

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.6/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.6/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.6/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.6/atlantis_linux_arm.zip)

# v0.3.5

## Features
* Log a warning if unable to update commit status. ([#84](https://github.com/runatlantis/atlantis/issues/84))

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.5/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.5/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.5/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.5/atlantis_linux_arm.zip)

# v0.3.4
## Description
This release delivers some speed improvements through caching plugins and
not running `terraform workspace select` unnecessarily. In my testing it saves ~20s per run.

## Features
* All config flags can now be specified by environment variables. Fixes ([#38](https://github.com/runatlantis/atlantis/issues/38)).
  * Completed thanks to @psalaberria002!
* Run terraform with the `TF_PLUGIN_CACHE_DIR` env var set. Fixes ([#34](https://github.com/runatlantis/atlantis/issues/34)).
  * This will cache plugins and make `terraform init` faster. Terraform will still download new versions of plugins. See https://www.terraform.io/docs/configuration/providers.html#provider-plugin-cache for more details.
  * In my testing this saves >10s per run.
* Run terraform with `TF_IN_AUTOMATION=true` so the output won't contain suggestions to run commands that you can't run via Atlantis. ([#82](https://github.com/runatlantis/atlantis/pull/82)).
* Don't run `terraform workspace select` unless we actually need to switch workspaces. ([#82](https://github.com/runatlantis/atlantis/pull/82)).
  * In my testing this saves ~10s.

## Bug Fixes
* Validate that workspace doesn't contain a path when running ex. `atlantis plan -w /jdlkj`. This was already not a valid workspace name according to Terraform. ([#78](https://github.com/runatlantis/atlantis/pull/78)).
* Error out if `ngrok` is already running when running `atlantis bootstrap` ([#81](https://github.com/runatlantis/atlantis/pull/81)).

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.4/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.4/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.4/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.4/atlantis_linux_arm.zip)

# v0.3.3

## Features
* Atlantis version shown in footer of web UI. Fixes ([#33](https://github.com/runatlantis/atlantis/issues/33)).

## Bug Fixes
* GitHub comments greater than the max length will be split into multiple comments. Fixes ([#55](https://github.com/runatlantis/atlantis/issues/55)).

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.3/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.3/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.3/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.3/atlantis_linux_arm.zip)

# v0.3.2

## Description
This release focused on some security issues reported by @eriksw, thanks Erik!
By default, Atlantis will be more secure now and you'll have to specify which repositories
you want it to work on.

## Features
* New flag `--allow-fork-prs` added to `atlantis server` controls whether Atlantis will operate on pull requests from forks. Defaults to `false`.
This flag was added because on a public repository anyone could open up a pull request to your repo and use your Atlantis
install.
* New mandatory flag `--repo-whitelist` added to `atlantis server` controls which repos Atlantis will operate on. This flag was added
so that if a webhook secret is compromised (or you're not using webhook secrets) Atlantis won't be used on repos you don't control.
* Warn if running `atlantis server` without any webhook secrets set. This is dangerous because without a webhook secret, an attacker
could spoof requests to Atlantis.
* Make CLI output more readable by setting a fixed column width.

## Bug Fixes
* None

## Backwards Incompatibilities / Notes:
* Must set `--allow-fork-prs` now if you want to run Atlantis on pull requests from forked repos.
* Must set `--repo-whitelist` in order to start `atlantis server`. See `atlantis server --help` for how that flag works.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.2/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.2/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.2/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.2/atlantis_linux_arm.zip)

# v0.3.1
## Features
* None

## Bug Fixes
* Run apply in correct directory when using `-d` flag. Fixes ([#22](https://github.com/runatlantis/atlantis/issues/22))

## Backwards Incompatibilities / Notes:
* None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.1/atlantis_linux_arm.zip)

# v0.3.0
## Features
* Fix security issue where Atlantis wasn't escaping the optional "extra args" that could be appended to comments ([#16](https://github.com/runatlantis/atlantis/pull/16))
  * example exploit: `atlantis plan ; cat /etc/passwd`
* Atlantis moved to new repo: `atlantisrun/atlantis`. Read why [here](https://medium.com/runatlantis/moving-atlantis-to-runatlantis-atlantis-on-github-4efc025bb05f)
* New -w/--workspace and -d/--dir flags in comments ([#14](https://github.com/runatlantis/atlantis/pull/14))
  * You can now specify which directory to plan/apply in, ex. `atlantis plan -d dir1/dir2`
* Better feedback from atlantis when asking for help via comments, ex. `atlantis plan -h`

## Bug Fixes
* Convert `--data-dir` paths to absolute from relative. Fixes ([#245](https://github.com/hootsuite/atlantis/issues/245))
* Don't run plan in the parent of `modules/` unless there's a `main.tf` present. Fixes ([#12](https://github.com/runatlantis/atlantis/issues/12))

## Backwards Incompatibilities / Notes:
* You must use the `-w` flag to specify a workspace when commenting now
  * Previously: `atlantis plan staging`, now: `atlantis plan -w staging`
* You must use a double-dash between Atlantis flags and extra args to be appended to the terraform command
  * Previously: `atlantis plan -target=resource`, now: `atlantis plan -- -target=resource`
* Atlantis will no longer run `plan` in the parent directory of `modules/` unless there is a `main.tf` in that directory.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.3.0/atlantis_linux_arm.zip)

# v0.2.4
## Features
* SSL support added ([#233](https://github.com/hootsuite/atlantis/pull/233))

## Bug Fixes
* GitLab custom URL for GitLab Enterprise installations now works ([#231](https://github.com/hootsuite/atlantis/pull/231))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.4/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.4/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.4/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.4/atlantis_linux_arm.zip)

# v0.2.3
## Features
None

## Bug Fixes
* Use `env` instead of `workspace` for Terraform 0.9.*

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.3/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.3/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.3/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.3/atlantis_linux_arm.zip)

# v0.2.2
## Features
* Terraform 0.11 is now supported ([#219](https://github.com/hootsuite/atlantis/pull/219))
* Safe shutdown on `SIGTERM`/`SIGINT` ([#215](https://github.com/hootsuite/atlantis/pull/215))

## Bug Fixes
None

## Backwards Incompatibilities / Notes:
* The environment variables available when executing commands have changed:
  * `WORKSPACE` => `DIR` - this is the absolute path to the project directory on disk
  * `ENVIRONMENT` => `WORKSPACE` - this is the name of the Terraform workspace that we're running in (ex. default)
* The schema for storing locks changed. Any old locks will still be held but you will be unable to discard them in the UI.
**To fix this, either merge all the open pull requests before upgrading OR delete the `~/.atlantis/atlantis.db` file.**
This is safe to do because you'll just need to re-run `plan` to get your plan back.

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.2/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.2/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.2/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.2/atlantis_linux_arm.zip)

# v0.2.1
## Features
* Don't ignore changes in `modules` directories anymore. ([#211](https://github.com/hootsuite/atlantis/pull/211))

## Bug Fixes
* Don't set `as_user` to true for Slack webhooks so we can integrate as a workspace app. ([#206](https://github.com/hootsuite/atlantis/pull/206))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.1/atlantis_linux_arm.zip)

# v0.2.0
## Features
* GitLab is now supported! ([#190](https://github.com/hootsuite/atlantis/pull/190))
* Slack notifications. ([#199](https://github.com/hootsuite/atlantis/pull/199))

## Bug Fixes
None

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.0/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.0/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.0/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.2.0/atlantis_linux_arm.zip)

# v0.1.3
## Features
* Environment variables are passed through to `extra_arguments`. ([#150](https://github.com/hootsuite/atlantis/pull/150))
* Tested hundreds of lines of code. Test coverage now at 60%. ([https://codecov.io/gh/hootsuite/atlantis](https://codecov.io/gh/hootsuite/atlantis))

## Bug Fixes
* Modules in list of changed files weren't being filtered. ([#193](https://github.com/hootsuite/atlantis/pull/193))
* Nil pointer error in bootstrap mode. ([#181](https://github.com/hootsuite/atlantis/pull/181))

## Backwards Incompatibilities / Notes:
None

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.3/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.3/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.3/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.3/atlantis_linux_arm.zip)

# v0.1.2
## Features
* all flags passed to `atlantis plan` or `atlantis apply` will now be passed through to `terraform`. ([#131](https://github.com/hootsuite/atlantis/pull/131))

## Bug Fixes
* Fix command parsing when comment ends with newline. ([#131](https://github.com/hootsuite/atlantis/pull/131))
* Plan and Apply outputs are shown in new line. ([#132](https://github.com/hootsuite/atlantis/pull/132))

## Downloads
* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.2/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.2/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.2/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.2/atlantis_linux_arm.zip)

# v0.1.1
## Backwards Incompatibilities / Notes:
* `--aws-assume-role-arn` and `--aws-region` flags removed. Instead, to name the
assume role session with the GitHub username of the user running the Atlantis command
use the `atlantis_user` terraform variable alongside Terraform's
[built-in support](https://www.terraform.io/docs/providers/aws/#assume-role) for assume role
(see https://github.com/runatlantis/atlantis/blob/master/README.md#assume-role-session-names)
* Atlantis has a docker image now ([#123](https://github.com/hootsuite/atlantis/pull/123)). Here is how you can try it out:

```bash
docker run runatlantis/atlantis:v0.1.1 server --gh-user=GITHUB_USERNAME --gh-token=GITHUB_TOKEN
```

## Improvements
* Support for HTTPS cloning using GitHub username and token provided to atlantis server ([#117](https://github.com/hootsuite/atlantis/pull/117))
* Adding `post_plan` and `post_apply` commands ([#102](https://github.com/hootsuite/atlantis/pull/102))
* Adding the ability to verify webhook secret ([#120](https://github.com/hootsuite/atlantis/pull/120))

## Downloads

* [atlantis_darwin_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.1/atlantis_darwin_amd64.zip)
* [atlantis_linux_386.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.1/atlantis_linux_386.zip)
* [atlantis_linux_amd64.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.1/atlantis_linux_amd64.zip)
* [atlantis_linux_arm.zip](https://github.com/runatlantis/atlantis/releases/download/v0.1.1/atlantis_linux_arm.zip)
