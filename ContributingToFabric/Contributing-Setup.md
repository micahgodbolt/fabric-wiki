## Advanced building tips

The repo contains many packages, each which may have dependencies on each other. You can use the rush tool to build projects in the correct order, if you have it globally installed.

```bash
npm install -g @microsoft/rush
```

To use rush to build, you can run `rush build`, which will incrementally build the entire repo (only build what has changed since the last build.)

To can also build up to a specific project using the `--to <package>` argument. For example, to build up to `office-ui-fabric-react`, you can run:

```bash
rush build --to office-ui-fabric-react
```






# Building the repo

Before you get started, **make sure you have read the [Git branch setup instrucions](Setup)**

To view the documentation including examples, contracts, component status, and to add functionality or fix issues locally, you can:

1. `git clone https://github.com/OfficeDev/office-ui-fabric-react.git`
2. `npm install`
3. `npm start`

This will start a demo page from the office-ui-fabric-react package folder, which will open a web browser with the example page. You can make changes to the code which will automatically build and refresh the page using live-reload.

To build and run tests for all packages in the repo, you can run `npm run build` from the root.

To build individual packages within the `packages/*/` folders, you can use `npm run build` in each individually. Note that because the packages are symlinked together, you must manage building dependencies in the right order, or use the `rush` tool to build to the specific package you want. (See advanced tips below.)