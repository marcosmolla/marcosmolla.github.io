---
layout: post
title: Updating Julia on your cluster
date: 2018-03-04 00:59:00-0400
description: Updating your version of Julia on a shared computer cluster
categories: sample-posts
giscus_comments: false
related_posts: false
---
When it comes to the console and using binaries, I am just not at all tech-savvy. I recently had to install an updated version of Julia on the computer cluster I am using (previously worked with an _[HTCondor](https://research.cs.wisc.edu/htcondor/)_ cluster, now I am at a _[SLURM](https://slurm.schedmd.com)_ cluster). I didn't want to break everyone else's programs, so I wanted to install it only for me.

The Julilang website kind of already gives you the answer [here](https://julialang.org/downloads/platform.html). But it still took me a while to figure our exactly what to do. So, here we go.

1. [Download the binaries](https://julialang.org/downloads/) from the Julialang website (in my case those were the Generic Linux Binaries for 64-bit architecture)

2. Upload this `.tar` file to your user folder on the cluster, e.g. `/home/yourusername/bin`

3. Untar the file using: `tar -xvf`

4. Change the folder name to something more useful (I just used the version number: `julia062`)

5. Now, we need to let the server know that it should this version of julia whenever we call julia from the terminal. To do this, we need to change the `PATH` variable; Have a look with echo $PATH (should look similar to this: `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games`); What we want to do, is to add our path `/home/yourusername/bin` to it. So, create a/or use the .profile file in your home directory (to get to the home directory, simply enter `cd`); with the command `nano .profile` a text editor opens; We want to add our path to the beginning of the `PATH` variable, so like in math we want the new `PATH` to be our `PATH` + `oldPATH`. To achieve this we will write: `PATH="/home/yourusername/bin/julia062/bin:$PATH"`; the `$PATH` simply adds whatever the `PATH` currently is to the end of our path. (You might have to replace `/home/yourusername` with `$HOME`, depending on the server settings). Finally, save the file by pressing `Ctrl-X` and accepting the changes.

6. Now, logout of the cluster and log in again.

7. When you now call julia, the version you just installed should be the one up and running. Remember, that you now also need to install all the packages you want to use for the updated version of Julia.

Enjoy your latest version of Julia!

Oh, and don't forget to change the call in your source files as well. Those look usually like this `#!/usr/bin/env julia` but now needs to be `#!/home/yourusername/bin/julia062/bin julia`. Otherwise, you will only use the latest version of julia whenever you call it directly, but your jobs for the cluster would still run with the version installed globally.

Still questions? Let me know how to improve this post.
