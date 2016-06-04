# OpenShift GitHub Tag

**Work In Progress: This is RDD: Readme Driven Development. So there is no code yet, just ideas for the moment.**

The idea is to automatically tag an image in OpenShift each time you tag your source code in GitHub.

For that, we would listen to [GitHub Push Events](https://developer.github.com/v3/activity/events/types/#pushevent) and for each new tag:

* check if there is a BuildConfig for the repository
* check if there is an existing Image for the commit of the tag
  * if no, trigger a new build for this commit
* tag the image with the name of the GitHub tag
* if the GitHub tag follows the [Semantic Versioning spec](http://semver.org/):
  * tag the image with the major version
  * tag the image with the major.minor version
  * tag the image with the major.minor.patch version

Example:

* Commit `7a42b13` and push
* GitHub triggers OpenShift build
* Git Tag version `1.2.3` (at commit `7a42b13`)
* Our little controller sees the tag event, and:
  * Tag the Image (result of the build for commit `7a42b13`) as `1`, `1.2` and `1.2.3`

Use-cases:

* Keep an environment that will be auto-redeployed at each commit: a DC with an ICT for the `latest` tag for example
* Have an environment that will be auto-redeployed for each new minor release: a DC with an ICT for the `1` tag for example
* Have an environment that will be auto-redeployed for each new patch release: a DC with an ICT for the `1.2` tag for example

With this controller, your OpenShift ImageStreams will automatically reflect the tagging system you have for your source code in git.
