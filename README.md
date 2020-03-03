# hApps design workshop

This is the source code for the [hApps design workshop](https://holochain-community-resources.github.io/design-workshop). It uses [Google's codelabs tools](https://github.com/googlecodelabs/tools) underneath.

## Editing

All steps in the workshop are defined and written in `design-workshop.md`. Edit that file to modify the workshop (reference: [Codelabs format guide](https://github.com/googlecodelabs/tools/blob/master/FORMAT-GUIDE.md)).

## Publishing 

This assumes you have [`claat`](https://github.com/googlecodelabs/tools/tree/master/claat) installed.

The workshop is hosted on Github pages. To publish a new version of the workshop, run this:

```bash
npm install
npm run build
npm run publish
```

Navigate to https://holochain-community-resources.github.io/design-workshop to see the changes (you may have to wait a couple minutes).