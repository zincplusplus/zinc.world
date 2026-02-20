---
title: Don't blame the wrong person
date: 2022-03-27
featured_image: /images/blog/22/mar/dont-blame-git-cover.png
excerpt: Want to know who actually wrote that line?
tags:
  - blog
  - code
  - git tricks
---

`git diff` is an incredible tool when navigating undocumented code. It allows me to navigate the deep Amazonian jungle that is the Booking.com codebase. I use it often to go deep and understand the reason behind a line of code and who wrote it, making it easy to ask for help or further clarification. But this all goes away when someone decides to fix the indentation or move some lines around. `git diff` by default shows the last person to touch a line of code, but not who made meaningful changes. Here's how to fix it.

## Better git diff in the terminal

When running `git diff` add `-w` to ignore whitespace changes and `-M` to skip moved lines. The resulting command should look like this `git blame -w -M -- path-to-file`.

Alternatively you can create an alias:

- Open your profile via `nano ~/.bash_profile`;
- Go to the end of the file and add the following line: `alias gblame="git blame -w -M"`;
- Press `Ctrl+x` to save and exit. When prompted to save type `y` for _yes_ and `Enter` to confirm;
- You can either restart the terminal or type `source ~/.bash_profile` for the change to take effect.

## Better git diff on Github

On Github ignoring whitespace changes boosts my productivity even more, especially when I'm reviewing a large pull request. Add `?w=1` to the URL or trigger it from the UI. Unfortunately, this preference doesn't stay.

<p class="text-center">
    <img src="/images/blog/22/mar/github-hide-whitespace-ui.png"/>
</p>

Good news for **Gitlab** users, though, where `?w=1` works took, and this behavior can be made permanent from your personal settings.

I hope this quick git trick will help boost your productivity even more. Happy coding.
