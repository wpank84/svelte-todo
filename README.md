# About This Repo

This repository was created as a way to provide a very quick overview of Svelte and Typescript in a simple CRUD application.

## Get started

### Fake REST API Setup

1. Run `npm install -g json-server`
2. Run `json-server --watch svelte-todo/db.json`

### Application

Install the dependencies...

```bash
cd svelte-todo
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.

If you're using [Visual Studio Code](https://code.visualstudio.com/) we recommend installing the official extension [Svelte for VS Code](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode). If you are using other editors you may need to install a plugin in order to get syntax highlighting and intellisense.
