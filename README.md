# Official Coinable Docs

[![Discord](https://img.shields.io/discord/952008855078973460?color=7289DA&style=plastic)](https://discord.gg/ykwpVayb)    [![Twitter](https://img.shields.io/twitter/follow/coinablepay?color=%231DA1F2&style=plastic)](https://twitter.com/coinablepay)

</div>
### Installation

```
$ yarn
```

### Local Development

```
$ yarn start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```
$ yarn build
```

This command generates static content into the `build` directory and can be served using any static contents hosting service.

### Deployment

Using SSH:

```
$ USE_SSH=true yarn deploy
```

Not using SSH:

```
$ GIT_USER=<Your GitHub username> yarn deploy
```

If you are using GitHub pages for hosting, this command is a convenient way to build the website and push to the `gh-pages` branch.
