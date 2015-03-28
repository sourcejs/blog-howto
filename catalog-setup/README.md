# Setting Up SourceJS Spec Catalogs

Version: 0.5.1

[SourceJS](http://sourcejs.com) Living Style Guide Plaftorm is focused on two important workflows - Modular UI Development and Living Style Guide Driven Development. Support of multiple libraries combination for different projects or just a set of standalone Front-end modules is one of the key features engine provides.

To make things clear, let's first define some main keywords.

* Component - standalone instance in your catalogue, could be a UI module (a button, web component, Angular directive), text documentation page, or a small CSS framework
* Spec - a documentation page, which describes your component; in SourceJS is rendered out of `index.src`, `readme.md`, `index.jade`, DSS doc or other custom formats you define
* Catalog/Bundle - Is a set of components with Specs, that you put in to SourceJS

Most typical SourceJS set-up that I've configured for multiple teams looks like this:

```
/sourcejs (Git repo)						- engine sources (don't need to modify)
	/core									
	/assets
	/docs
	/user (Git repo)	 					- SourceJS instance configuration and content repo
		/core								- overriding engine core
		/assets							- overriding engine assets
		/specs								- default catalog folder for Specs 
			/bootstrap (Git repo)			- nested bundle with Bootstrap components
				/bootstrap-component1
					index.src				- default template for describing component
					info.json	
				/bootstrap-component2
					...
				index.src					- default template for describing Nav page
				info.json					- essential Spec or Nav page config file
				
			/project-bundle (Git repo)		- nested project specific bundle
			/component1	 (Git repo)			- nested standalone component
				index.src					
				info.json					
				
			/component2 (Git repo)
				README.md					- alternative MD template for describing component
				info.json					
		
```

Following official [engine setup instructions](http://sourcejs.com/docs/base/#install) after running `yo sourcejs` setup process, you will get `sourcejs` folder with Git repo attached. It is safe to clean `.git` folder, but we suggest keeping this repo initialized for easy updates via `git pull`.

After set-up, apart from engine sources, you get personal configuration folder `sourcejs/user`. There you define custom options, override engine views and keep your documentation contents.

We suggest setting up personal version control repo in `sourcejs/user` folder, so you can share your setup with the rest of the team.

## Catalogs and Bundles

SourceJS supports multiple catalog system. It means you can create deep nesting level with different bundles inside. Default init script creates `sourcejs/user/specs` folder with first Spec example. You can rename `specs` folder, and use your own catalogs and freely nest other catalogs and sub-repos inside.

As mentioned in the basic setup file scheme above, it's recommended to keep UI component bundles in separate repos. Here are few example bundles used in [sourcejs.com](http://sourcejs.com) setup:

* https://github.com/sourcejs/example-bootstrap-bundle
* https://github.com/sourcejs/example-specs-showcase

## Preparing Nested Bundles

The only main requirement for SourceJS compliant bundles and Specs is `info.json` file, where you keep meta information about documentation or navigation pages. Additionally to it, you will also need to provide `README.md` or `index.src` files to define Spec contents for the engine to render.

### Catalog Level Navigation

If you want to have a bundle level navigation like in [Example Specs](http://sourcejs.com/specs/examples/) section, set-up a simple `index.src` template following [generated nav doc](http://sourcejs.com/docs/data-nav/).

```html
<div class="source_subhead">
    <h1>My custom bundle</h1>
</div>

<div class="source_catalog" data-nav="bundle-folder-name" data-tag="without-tag">
    <h2 class="source_catalog_title">Bundle navigation title</h2>
</div>
```

And define the `info.json` meta configuration file next to it.

```json
{
    "title": "My custom bundle",
    "role": "navigation"
}
```

Check [example bundle root](https://github.com/sourcejs/example-specs-showcase) and compare it with [rendered result](http://sourcejs.com/specs/examples/).

### Individual Component Set-up

To render a Spec page per component, and list it in navigation page, `info.json` file must be added to each item:

```
/bundle
	/component1
		component.css
		component.js
		index.template
		info.json
		readme.md
	/component2
		component.css
		info.json
		index.src
```
Check out  `index.src ` [Spec example](https://github.com/sourcejs/example-specs-showcase/tree/master/default).

## Organizing Nested Bundles

Feel free to store all component sources and documentation right in `sourcejs/user` folder. But if you want to re-use some of the components across different projects, it's better to organize each bundle in separate repo.

### Git Submodules

One of the easiest options to configure nested repos, is to use Git submodules. To define a submodule, run this command from parent repo in `sourcejs/user`:

```
git submodule add https://github.com/sourcejs/example-bootstrap-bundle.git specs/example-bootstrap-bundle
```

To update all existing submodules run:

```
git submodule foreach git pull origin master
```

This set-up helps keeping all catalogs in one place. Using `git clone --recursive` other team members will get the same sub-repo structure with all SourceJS contents.

Read more about Git Submodules in [official specs](http://git-scm.com/docs/git-submodule).

### Symlinks

If you prefer flat tree, instead of submodules use symlinks:

```
git clone https://github.com/sourcejs/example-bootstrap-bundle.git
ln -s example-bootstrap-bundle/ sourcejs/user/specs/example-bootstrap-bundle
``` 