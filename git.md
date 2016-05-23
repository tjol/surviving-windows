# git on Windows

`git` can work tolerably well on Windows these days. It even comes with bash and vim!
(Because really, you can't use `git` on Windows without pretending it's UNIX...)

There are problems though.

### Permissions

Windows doesn't have UNIX file modes. MSYS (which Git for Windows uses) and Cygwin get
around this in incompatible ways, which can create major headaches.

**Note:** It's best to use *either* MSYS-git *or* Cygwin's git, but not both. Actually,
use the standard MSYS-git! Cygwin makes things too complicated.

Sometimes, something makes git want to **mess up file modes for no good reason**. Constant
vigilance! If this happens, it can be fixed:

    git update-index --chmod=-x <file> # or similar

As git's simulated file modes are somewhat buggy and thoroughly useless, it might be best
to just disable the whole thing:

    git config core.filemode false

This and more [found on stackoverflow](stackoverflow.com/questions/6476513/git-file-permissions-on-windows).

### CRLF

Classic pain in the arse. Normally it doesn't matter *that* much: most software on most
platforms can handle any type of line ending just fine. However:

**If you want UNIX #! lines to work, make sure your scripts use life feeds ONLY!** 

The classic configuration flag is:

    git config --global core.autocrlf true

The *can* be set on a per-repository basis. For particular file types, it can also be
enforced in `.gitattributes`, for example:

    $ cat .gitattributes
    *.py eof=lf

More details can be found [in the docs](https://help.github.com/articles/dealing-with-line-endings/).

### Mapped network drives

Cloning a repository on a mapped network drive the usual way may not work (God knows why).
The solution (apparently) is to clone from a `file://X:/Y/Z` URL to a local path. *Par exemple,*

    $ git clone pump_probe/ pump_probe2
    Cloning into 'pump_probe2'...
    done.
    error: internal error: refs/remotes/origin/master is not a valid packed reference!
    fatal: update_ref failed for ref 'HEAD': cannot update the ref 'HEAD': Trying to write ref refs/heads/master with nonexistent object 05f2ce3c7bfa9c764659c426fd58a5f15e1e2033
    fatal: The remote end hung up unexpectedly

fails while 

    $ git clone file://P:/code/py/pump_probe pump_probe2
    Cloning into 'pump_probe2'...
    remote: Counting objects: 61, done.
    remote: Compressing objects: 100% (60/60), done.
    remote: Total 61 (delta 19), reused 0 (delta 0)
    Receiving objects: 100% (61/61), 5.47 MiB | 7.76 MiB/s, done.
    Resolving deltas: 100% (19/19), done.
    Checking connectivity... done.
    Checking out files: 100% (28/28), done.

works.

What a bizarre world. Solution found [in the MSYSgit forum](https://groups.google.com/forum/#!msg/msysgit/5J3ELvZND0s/2VPm-eUf0YMJ).

