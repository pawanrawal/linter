# lint

We all know well linted code makes for good code and keeps everyone happy. lint is a linter which constantly monitors your pull requests on Github, finds out lint errors for your Go code and adds them as comments on your PRs. We came up with the idea of lint from our own experience at Dgraph. We wanted a way to incorporate linters like go vet, golint and others into our development process. As we follow code review process deligently, we thought about integrating the linting errors into our reviews. Once the errors were there, the reviewer and the developer could decide to fix them or not. We used [houndci](https://github.com/houndci/go) for a while but found that it lacked even the most primitive features. With **lint** you can
  - Ignore autogenerated files and folders
  - Run your file through multiple linters (Support for https://github.com/kisielk/errcheck and https://golang.org/cmd/vet/ is part of the Roadmap)

## Setup

* First step is to generate a personal access token for the repository you want to run this for using https://help.github.com/articles/creating-an-access-token-for-command-line-use. While creating the token, add the **admin:repo_hook** scope to it.
* Then you'd want to create a webhook which can send updates about pull request creations/updates to our server like mentioned https://developer.github.com/webhooks/creating. You would just want to subscribe to the **pull_requests** events because thats all that we need.
* Finally as mentioned https://developer.github.com/webhooks/configuring you'd want to configure your server and add the server url to the webhook. [ngrok](https://ngrok.com/) is a really cool tool that allows you to expose your local server over http/https.

## Usage

Once you have the webhook setup and the access token with you, you can start lint like

`go build && ./dlint -token=token -repo=https://api.github.com/repos/pawanrawal/ideal-octo-fortnight -ignore=lintignore`

`./dlint --help` tells us what flags are available

```
Usage of ./dlint:
  -debug
           In debug mode comments are not published to Github
  -ignore string
           Name of file which contains information about files/folders which should be ignored
  -port string
           Port to run the web server on. (default ":4567")
  -repo string
           basePath for repo
  -token string
           Access token for the Github API
```

You can supply dlint with an ignore file with the `-ignore` file. A sample ignore file is like

```
// Do not lint files in the vendor folder. The trailing / is important when you want to ignore all files in a folder.
vendor/
// Do not lint files generated by protocol buffer compiler.
*.pb.go
// Do not lint this particular file.
folder/ignore.go
```
## Contributing

Contributions in the form of issues/pull requests are welcome. Please create a Github issue for any bug that you find.

