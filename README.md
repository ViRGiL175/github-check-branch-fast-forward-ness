# What's it?

It's a simple composite Action in Bash, which checks if the Branch from Pull Request was merged into the main Branch where it was targeted into.

Action is performing the similar functional as the “Require branches to be up to date before merging” feature on GitHub. But this feature is not available for the private repositories without paid subscription.

![image](https://user-images.githubusercontent.com/11541555/134152498-13acb13e-a9c1-492b-a706-af3fff5aaa8c.png)

You can find the similar native function on GitHub in section Branches in repository's settings.

# What for?

Using this method can reassure you that the Pull Request Branch was tested in the context of your main Branch (or at least it is ready for launching and testing).

# How does it work?

Pretty simple and fast ≈ 6 ms.

Action is checking Pull Request Branch's fast-forward-ness, which means it can be easily merged, or ready to be merged with your main Branch. To do this, Action uses this native git command:

```
$ git merge-base --is-ancestor
```

If the command is a success, everything is great and Action shows you 👍.

If the Pull Request Branch is not updated according to your main Branch, Action finishes with a error and shows you a pretty git log of your branches's tree, so you can investigate your problem it in place.

# How can I use it?
It's really simple. **But it's only for Pull Requests workflows!**

Create file `check-fast-forward-ness.yml` in `.github/workflows` directory:

```yaml
name: Check branch fast-forward-ness

on:
  pull_request:
    branches: [main]
    types: [review_requested]

jobs:
  check-fast-forward-ness:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ViRGiL175/check-branch-fast-forward-ness@main
```

Now your Pull Requests will be tested on fast-forward-ness.

If your remote's name isn't "origin", you can use `remote-name` parameter to set the custom remote's name:

```yaml
name: Check branch fast-forward-ness

on:
  pull_request:
    branches: [main]
    types: [review_requested]

jobs:
  check-fast-forward-ness:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ViRGiL175/check-branch-fast-forward-ness@main
        with:
          remote-name: different-remote-name
```

# What's next?

Feel free to do any Forks and Pull Requests! For sure there are tonnes of bugs and there is much to improve.

> As far as I know, composite actions have some restrictions now, especially in the field of error handling. As the composite runner is developing, the code of this small Action can be made more useful and neat.

# Links

Inspiration on branch fast-forward-ness: </br> [https://gist.github.com/briceburg/3f41f09bdc478d21bcf8](https://gist.github.com/briceburg/3f41f09bdc478d21bcf8)

My alias for the nice performing of the commit tree: </br> [https://gist.github.com/ViRGiL175/fad0e017c4bb638584c7233717b5122b](https://gist.github.com/ViRGiL175/fad0e017c4bb638584c7233717b5122b)
