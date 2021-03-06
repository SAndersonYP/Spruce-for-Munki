![Picea Abies](http://www.imagines-plantarum.de/img/0711907.jpg)

# Spruce
Over time, and, especially with the use of AutoPkg, your Munki repo has
probably accumulated some cruft: piles of test pkginfos and out-of-date Chrome
updates, for example. Spruce looks for objects in your repo which have no
current usage, are out of date, or are otherwise "crufty".

## What Can Spruce Do?
With Spruce you can quickly generate a list of all of the unique `name`s
of products in your repo (optionally listing each version of that
product).

You can get reports on products that are not in any manifests, how much
wasted drive space they take up, and see reports of less-than-current
versions of software.

Spruce can generate lists of all products with a given `category`. It can
output complete category information in plist form, and then recategorize
products in bulk. For example, if you have three categories named
"Config", "Configuration", and "Configuration Items", you could easily
merge the three into one category.

Spruce can remove package and pkginfo files simply by product name, or
even by category (for example, a "To Remove" category). When Spruce
removes a product, it removes every pkginfo file that uses that `name`,
as well as all referenced pkg/dmg files. It will also remove all uses of
that `name` in your repo's manifests.

Optionally, you can move these files to a repo archive instead of
deleting them. The archive folder will have the same structure as the
active repo.

Finally, Spruce can generate a list of icons not used by any products,
following the guidelines for icon file searching specified in the Munki
documentation. You can optionally have Spruce delete or archive all
unused icons as well.

With any of the removal or archive features, it behooves the user to have
either a backup or to have the repo in version control.

(Blog post coming about this topic).

## Usage
*This is very much beta!*


```
usage: spruce [-h]
              {name,report,category,recategorize,deprecate,icons,docs} ...

Spruce is a tool for improving the quality of your Munki repo.

positional arguments:
  {name,report,category,recategorize,deprecate,icons,docs}
                        Sub-command help
    name                Output all unique product names present in the Munki
                        all catalog.
    report              Report on unused or misconfigured items in the repo.
    category            List all categories present in the repo, and the count
                        of pkginfo files in each, or show members of a single
                        category.
    recategorize        Recategorize products based on an input plist
                        generated by the prepare command.
    deprecate           Remove unwanted products from a Munki repo. Pkg and
                        pkginfo files will be removed, or optionally can be
                        archived in an archive repo. All products to be
                        completely removed will then have their names removed
                        from all manifests.
    icons               Report on unused icons and optionally remove or
                        archive them.
    docs                Generate markdown documentation from configured Munki
                        repo.

optional arguments:
  -h, --help            show this help message and exit
```

Subcommands have further options, which you can learn about by running Spruce with the -h command, like this: `./spruce.py icons -h`.

Obviously this is a powerful and dangerous tool. You've been warned!

## TODO
- Report options, allowing you to run a subset of all reports. 
- Documentation!
- Move verb: Allows you to simultaneously move a pkginfo and its pkg to a new folder in the repo. (HINT: This is basically deprecate, minus the manifest manipulations).
- As part of move/deprecate implementation, also handle arbitrary random crap in the repo, as well as icons, client_resources, just pkgs (without a reference from a pkginfo).
- Search verb: Look through pkgsinfo, pkgs, manifests, icons, etc, for search term and provide formatted results.
- Report for items which share a `name`, but have differing `category`, `developer`, etc values.
- Report for unused client_resources.
- Report for "misplaced" crap in your repo. (i.e. some random txt file in your pkgs).
- Circular dependency report.
- Cruftiness ranks for reports.
- Git-aware deprecate features (git rm for pkginfo, git rm for pkgs, individually configurable).
