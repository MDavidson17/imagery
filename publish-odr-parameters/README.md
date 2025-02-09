# Workflow Parameters

This folder contains the parameter yaml files used when running the `publish-odr` argo workflow. This workflow publishes imagery to the [Registry of Open Data](https://registry.opendata.aws/nz-imagery/) and is considered a data release mechanism.

## How it works

This process ensures that at least two sets of eyes review each dataset before it is published `s3://nz-imagery` (the ODR).

### Step 1: Create Parameters Yaml file/s

**example.yaml:**

```yaml
source: 's3://linz-imagery-staging/test/'
target_bucket_name: 'linz-imagery-staging'
```

_nb: a [new script](https://github.com/linz/argo-tasks/blob/master/src/commands/path/path.generate.ts) has been created which automatically generates the target path using the collection metadata. In future, this release process will be refactored so that the target path can be visualised and accepted prior to being published to the ODR_

Additional parameters can be added, more information can be found [here](https://github.com/linz/topo-workflows/tree/master/workflows/imagery#publish-odr).

**Save this file to the `publish-odr-parameters` folder** - if saved in any other location the workflow will not run, subdirectories are ok.

### Step 2: Create a Pull Request

Create a pull request to this repo, include the key information for the reviewer.

_nb: after some production iterations we will work to automate the creation of a draft PR that will bring across the appropriate links_  
Below is an example PR:

```md
# Publish to ODR

- Dataset Title: _paste here_
- Basemaps Config Link: _paste here_

## Reviewer Checklist

- [ ] Basemaps Config
- [ ] Target Path
- [ ] collection.json metadata
```

### Step 2a: if you dont want submit workflows

There may be some cases when you dont want to submit workflows, for example reformatting files.  
In this case add `[skip-argo]` to the end of the pull request title e.g- `fix: Reformatting parameter files [skip-argo]`

### Step 3: Approve and Merge Pull Request -> Submit Argo Workflow (Automated)

Once the pull request has been reviewed, approved, and merged the [publish-to-odr](../.github/workflows/publish-to-odr.yml) Github Action will run. This will sumbit a `publish-odr` argo workflow for the added or modified parameter files.

## Testing publish-to-odr.yml

If making changes to the github actions please **don't** test the action using `publish-odr` and an `s3://nz-imagery` target, instead change the action to call `publish-copy` and use a temporary target like shown in the example above.
