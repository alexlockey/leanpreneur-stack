# Updating your skillpack

Own your copy. Pull the upgrades you want. Lose nothing you wrote.

When you take a skill bundle from the Stack (or you get an install built on it), you own that copy outright. Edit it freely. Change the prompts, add your own skills, rename things to fit how you work. It is yours.

The question this page answers: when the Stack later improves one of those skills, how do you take the improvement without losing the edits you made? The answer is a reference diff.

## The two moves

### 1. Scaffold

You copy a bundle into your own workspace. A plain copy or a `git clone` is all this is. From that moment, the copy is yours. There is no managed block, no locked file, nothing that reaches back in and overwrites your work. You edit anything you like.

### 2. Reference

Later, the Stack ships a better version of a skill. You want the improvement, but you have your own edits in that bundle and you do not want to throw them away. So instead of re-copying the whole thing, you run a reference diff. It compares your copy against the new upstream version and tells you, file by file:

- **New upstream** files you have never seen. Safe to add.
- **Upstream improved** a file you did not touch. A clean pull, you lose nothing.
- **You edited** a file upstream did not change. Keep yours, there is nothing to pull.
- **Conflict**: both you and upstream changed the same file. This is the only case that needs you to merge by hand.
- **Your own** files, not in upstream at all. A pull never touches them.

You read the diff. You pull what you want. You skip what you do not. Nothing happens automatically and nothing is ever written into your copy by the tool. You stay in control of every change.

## Why it works this way

Most "update" mechanisms overwrite. You run the update, your local edits are gone, you find out the hard way. That is the opposite of owning your copy.

A reference diff flips it. It never writes into your copy. It produces a review, and you do the pulling by hand. The cost is sixty seconds of reading a diff. The benefit is you never lose your work and you are never stranded on a frozen version. You take the upgrades on your terms.

## The manifest

Each bundle carries a small `bundle.json` manifest. It records the bundle name, version, the file list, and a snapshot of the last upstream version you synced from. That snapshot is what lets the diff tell the difference between "upstream changed this" and "you changed this." You do not edit the manifest by hand. The tool stamps it.

After you pull a set of upgrades, you re-stamp the manifest so it knows the new baseline. Then the next diff only shows you what is new since then.

The manifest also names the model tier a skill prefers (`precision`, `default`, `fast`, or `research`), never a specific vendor's model. That keeps your bundle portable: it runs on whatever model provider you point it at, and the only file that maps a tier to a real model is the shim in your own setup. See the portability note in the main README.

## How to run it

If you are running this inside an AFCS-style setup, the tool is `tools/run-skillpack-reference.py`.

Stamp a manifest on your copy the first time, recording where you synced from:

```
python3 tools/run-skillpack-reference.py manifest \
  --bundle path/to/your/bundle \
  --name your-bundle-name --version 1.0 \
  --record-synced-from path/to/upstream/bundle \
  --live
```

Later, when you want to see what is new upstream:

```
python3 tools/run-skillpack-reference.py reference \
  --owned path/to/your/bundle \
  --upstream path/to/upstream/bundle
```

That prints the review and writes nothing. Add `--live` to also save the review to a file. Even with `--live`, the tool only saves the review, never your bundle. Your copy is read-only to the tool, always.

Then pull by hand, following the checklist the review prints at the end:

1. Copy the new upstream files you want.
2. Replace your copy with upstream for the clean-pull files you want.
3. Merge any conflicts by hand.
4. Leave your own files and your edited files alone.
5. Re-stamp your manifest so the next diff has a fresh baseline.

That is the whole model. You own your copy, you see exactly what changed, and you take the upgrades you want without losing the work that makes the copy yours.
