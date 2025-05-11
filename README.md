# Strong Warning

It is my personal view that this is a [RTFM](https://en.wikipedia.org/wiki/RTFM) honey-trap. 

Do not be taken in by description of it's functionality as "Quick" and "Simple".

## Example

Attempting to remove a large file, so that unstaged changes can be pushed to origin:

```zsh
git filter-repo --path path/to/large/file.mp4 --force
NOTICE: Removing 'origin' remote; see 'Why is my origin removed?'
        in the manual if you want to push back there.
        (was git@github.com:you/your-repo.git)
Parsed 173 commits
New history written in 0.34 seconds; now repacking/cleaning...
Repacking your repo and cleaning out old unneeded objects
HEAD is now at d0c5b4c save
Rewrote the stash.
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (7/7), done.
Total 7 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Completely finished after 7.96 seconds.
```

## ⚠️ Consequences

- ⚠️ All unstaged work irrepairably deleted
- ⚠️ Only `path/to/large/file.mp4` is preserved
- ⚠️ Only files in origin are preserved

## "Old Unneeded Objects"

```python
    if (repack and not run_quietly and not show_debuginfo):
      print(_("Repacking your repo and cleaning out old unneeded objects"))
    quiet_flags = '--quiet' if run_quietly else ''
    cleanup_cmds = []
    if repack:
      cleanup_cmds = ['git reflog expire --expire=now --all'.split(),
                      'git gc {} --prune=now'.format(quiet_flags).split()]
    if reset:
      cleanup_cmds.insert(0, 'git reset {} --hard'.format(quiet_flags).split())
    location_info = ' (in {})'.format(decode(repo)) if repo != b'.' else ''
    for cmd in cleanup_cmds:
      if show_debuginfo:
        print("[DEBUG] Running{}: {}".format(location_info, ' '.join(cmd)))
      ret = subproc.call(cmd, cwd=repo)
      if ret != 0:
        raise SystemExit("fatal: running '%s' failed!" % ' '.join(cmd))
      if cmd[0:3] == 'git reflog expire'.split():
        self._write_stash()
```

Use at your peril, or become a statistic.

