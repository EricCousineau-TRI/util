# util

Random bash utils, some adapted from other peeps' stuff. Primarily

*	git-submodule-ext - Submodule extensions. See `SUBMODULES.md` for more info.
*	git-new-workdir - Modified from git/contrib to work for submodules
*	git-emeld - Editable version of git-meld using git-new-workdir
*	run-or-raise - Basic implementation (should use other one ??? )

# Setup

	sudo ./install /usr/local/bin
	./aliases # Set some git aliases

If you wish to develop on them while keeping in your `$PATH`, then do

	./install --link ~/local/bin
	./aliases

To set up auto completion to link to your development version, add it to `~/.bash_completion`:
	
	#!/bin/bash
	source /path/to/git-util/git-sube

# Credits

*	Jens Lehmann, Junio Hamano, Heiko Voigt, Phil Hord - Design guidance and suggestions and for modifications to submodule foreach.
*	Julian Phillips, Shawn O. Pearce - Original git-new-workdir
*	Antonin Hildebrand - [Discussion](http://comments.gmane.org/gmane.comp.version-control.git/196019) for git-new-workdir with submodules
*	wmanley - Original [git-meld](https://github.com/wmanley/git-meld)
*	Vicky Chijwani - run-or-raise [blog post](http://vickychijwani.github.io/2012/04/15/blazing-fast-application-switching-in-linux/)
*	David Mikalova - better run-or-raise implementation, [brocket](https://github.com/dmikalova/brocket.git)

# Todo

*	Add `git-submodule-ext dir-sync` with two options:
	1.	`super` - Will ensure that all submodule `$GIT_DIR`'s are in the supermodule's `$GIT_DIR/modules/$path`
	2.	`repo` - Will ensure that all submodule `$GIT_DIR`'s are in the submodule's `$path/.git`
*   Have `git-submodule-ext` support cloning a repository, where multiple submodules from the same effective repository. Do not clone the same repostory multiple times if appropriately marked.
    *   Should be robust against different URLs
    *   Should identify the ground truth submodule via config, leveraging `git-new-workdir` (e.g., `git config submodule.${new_workdir}.orig_workdir`)
    *   `orig_workdir` will point to the worktree path to maintain integrity; it will NOT point to the `GIT_DIR`
    *   For nested submodules, should ensure that `orig_workdir` has its submodules intialized to enable them to be `git-new-workdir`'d as well
    *   Should only track this where `orig_workdir` is underneat the supermodule's worktree
    *   Example:
        *   `sandbox/`
            *   `drake-distro/` - For primary development
            *   `branches/drake-distro/`
                *   `issues/5464_return_binding` - `new_workdir` from `:/drake-distro`
                *   `feature/warm_start_prototype` - `new_workdir` from `:/drake-distro`
