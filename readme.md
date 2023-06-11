# git-submodules-troubleshooting
Is this useful: [Buy me a coffee](https://www.buymeacoffee.com/talwrii/)

I have worked with and created and maintained a number of projects with using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) and value the feature 
and would class myself as an "expert" or at least "proficient" git user and yet every so often when I hit bugs with git submodules that I find irritating. 
This derives from a lack of undertanding of how git submodules work, but my googling around the topic often results with "how-to pages" telling me how
useful git submodules is without really giving me an understanding. 

This page of limited scope tries to give a git submodules user sufficient understanding to fix their git submodules problems, while also focusing on submodules. 

# References
1. The [official documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
2. [This useful cheatsheet](https://www.devroom.io/2020/03/09/the-git-submodule-cheat-sheet/); does not really discuss how things work under the hood.

# Confusing things about `git submodules`
Git submodules is a layer that runs on top of git - this is confusing because it is "half git / half something else" and understanding git submodules require a measure  of undestanding of both.

Git submodules often "just works" enough that you don't need to understand the internals - this means that you don't naturally buid uip an understanding of git submodules through us. 

Despite mostly "just working" sometimes certain things you need to do (particularly when they go wrong) aren't abstracted over.

# How git submodules actually works
1. A commit object is stored in the working tree at a location
1. An entry in `.gitmodules` contains information about where git submodules should find this commit. This is committed and shared with other people.
1. An entry in `.gitconfig` contains information about what is actually checked out (this information is not committed) and allows for tweaks.

# Cookbook
## I updated `.gitmodules` but my repository but git clone still doesn't work
The repository url is cached in .gitconfig. Use [your editor](#git-config) to remove it from `.git/config`.

## `git submodule update` hangs on `Cloning into...`
Potentially repository that you are cloning from does not exist. You might need to get it running again, or change it. 

Proper fix:
1. Go to `.gitmodules` check the `url` entry for the submodule in question
2. Check that this can be checked out, if not update the url
3. Remove the [submodule] url parameter for this module from `.git/config`
4. Run `git submodule init $MODULE` to create a new entry `git/config`
5. Run `git submodule update $MODULE` to check out the git module

## I want to remove a git submodule

1. Commit .gitmodules `git add .gitmodules; git commit`
2. git rm submodule (this apparently automatically updates .gitmoudles)

## Editing `.git/config` is too slow
<a name="git-config" />

You can use `git config`

* `git config | grep submodule`
* `git config --unset submodules.$path`
