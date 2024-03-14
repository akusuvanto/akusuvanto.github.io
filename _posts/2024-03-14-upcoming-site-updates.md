---
layout:     single
title:      "Upcoming site updates"
date:       2024-03-14 06:00:00 +0300
permalink:  /2024/03/upcoming-site-updates
categories:
  - Site Updates
tags:
  - Jekyll
  - Web Development
---

Long time no see! And of course happy Pi Day as well. 
It has been quite a while since this page last had any new content posted but it is finally time to dust off the repo and get back into the swing of things!

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/2024-03-14/analogicus-cranes.jpg" alt="Two tower cranes with a light blue sky as  the backdrop">
  <figcaption>You don't usually use cranes to build websites but they do still look pretty nifty. Picture by <a href="https://pixabay.com/users/analogicus-8164369/">Analogicus</a></figcaption>
</figure> 

First things first of course, I had to get a local copy of the site running to continue. As it had been quite a while since last working on the project, I had moved to a new development environment and needed to do some setup.

I proceeded to install Ruby and Bundler to the system and then cloned the existing repo from Github. Next, everything was supposed to be as simple as running <code>bundle install</code> to get all the required Ruby Gems specified by the project Gemfile but bundler would fail with a permission error.

Manually installing Gems worked, so I decided to try running bundler with elevated privileges. It worked and all the packages got installed, however bundler gave me a warning saying it should never be run using <code>sudo</code>. A little late for the warning perhaps. The reason turns out to be that when installing gems, bundler will assign file permissions according to the user who ran bundler which will in the case of sudo, according to bundler, break the application for all non-sudo users. That is not quite what I wanted, so after a few Google searches I found a fix, apparently the magic words <code>sudo bundle install --system</code> would fix this little mistake and reassign the permissions correctly.

The proper solution, which I later applied, was to set <code>$BUNDLE_PATH</code> and <code>$GEM_HOME</code> somewhere where your user has the appropriate file permissions. Running <code>export BUNDLE_PATH=~/.gems</code> and <code>export GEM_HOME=~/.gems</code> would tell bundler to install all gems to a hidden folder in the user's home directory.

One <code>bundle install</code> later running <code>bundle exec jekyll serve</code> would finally serve the local copy. Excellent! A few config changes later Jekyll would now build the site using the latest release of the installed theme, Minimal Mistakes 4.24.0... which seems to no longer be under active development with the last commit being from over 2 years ago. Uh oh.

Well not really, nothing is broken, everything builds just fine. However, seeing that did light the little spark that will often turn into a side project. The beauty of markdown is that it is portable. I could easily install another theme... or better yet, build something new that can render the same old markdown onto a way snazzier site. 

This site is a personal project, switching things up to learn is how it should be! In the coming few weeks I'm planning to build a new site and migrate the existing content there so if this post is older than that, you might already be reading it there! I'm also planning on writing a few posts about the process, which of course will be available here and later on the new site as well.

Thanks for reading!