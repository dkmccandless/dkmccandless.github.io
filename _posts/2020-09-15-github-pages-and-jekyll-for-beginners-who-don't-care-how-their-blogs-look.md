---
layout: post
title: Github Pages and Jekyll for Beginners Who Don't Care How Their Blogs Look
date: 2020-09-15 22:03:47 +0800
---

Blogging is one of the creative endeavors that has long tugged at me, but the inspiration I've felt to contribute my observations has always been tempered by trepidation and second-guessing on account of the catch-22 of needing experience to benefit from exposure—which is, of course, one of the currencies of experience.

So, in the interest of learning to swim by throwing myself into the deep end of the pool, I signed up for the Iron Blogger Challenge when it was organized at the beginning of my RC batch. In setting up my site, however, I was blocked for a long time by the bewildering complexity of the blogging service ecosystem. Before even starting up, the amount of reading and research necessary just to make an educated choice between a small slice of the available services frustrated me, particularly since I have never had much visual sense, nor cared what the things I made looked like (nor had any intuition for how to present them with a pleasing aesthetic); all I wanted was to put words somewhere people could see them if they cared.

For any who would follow in my footsteps, here's what I did to get started. 

### ~~A How-To~~ ~~An Experience Report~~ A Cautionary Tale

I settled on [Github Pages](https://pages.github.com/) because I'm comfortable with the mechanics of Git and GitHub, "dkmccandless.github.io" is a pretty straightforward URL to have a blog at, and "GitHub Pages publishes any static files that you push to your repository" has a certain ring to it.

There seem to be numerous advantages to publishing posts in the form of static pages, and a corresponding subset of the blogging ecosystem is devoted to creating and integrating them. [Jekyll](https://jekyllrb.com/) is a popular static site generator service, perhaps largely because of its customization options, but also, and more importantly in my case, for being [directly integrated with GitHub Pages](https://docs.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll). The fewer extraneous complexities, the better...or so I thought. But wow, did I have a lot to learn, both figuratively and literally. Publishing sources, liquid, front matter, some of the less savory vagaries of HTML, YAML, the list goes on.

Or so I thought. The challenge of balancing depth-first and breadth-first organization of new knowledge when learning a new practical topic completely from scratch can be a significant one, can't it. After hours upon hours of yak shaving, I learned a couple of things that obviated a lot of the complexity for me: first, that GitHub Pages automatically publishes static pages as long as you've correctly initialized the [publishing source](https://docs.github.com/en/github/working-with-github-pages/about-github-pages#publishing-sources-for-github-pages-sites) branch and folder of your repository and pushed them there, and second, that the settings contain an integrated "theme chooser" that will just make your pages look some nice way or other, if you write your pages in [Markdown](https://guides.github.com/features/mastering-markdown/). Needless to say, this is exactly what I'd been looking for.

Except for a couple of issues. For starters, GitHub Pages was "disabled" until I pushed something to the repository, so I did: following the site's documentation on setting up with Jekyll, I converted the first post I had written to Markdown, committed it, and pushed it. (This mistake would come back to haunt me later; I could have avoided it by reading about how [Posts](https://jekyllrb.com/docs/posts/) work on Jekyll before following the GitHub Pages setup instructions.) GitHub Pages then proudly announced that my site was "ready to be published". Except not really, because surprise!, an index.html file is required. More on that later.

Meanwhile, getting Jekyll and Bundler to work was also a stumbling block. When I tried to create a new Jekyll site in my intended publishing source according to GitHub's instructions, I was met with a frustrating error:

    $ bundle exec jekyll 3.9.0 new .
    Could not locate Gemfile or .bundle/ directory

This message probably makes sense to anyone with enough experience with Ruby. Not me though. Searching online indicated that many people have asked about this error, but a fix was not forthcoming: speculations on Jekyll Talk, the Issues section on Jekyll's GitHub repository, and Stack Overflow were mostly concerned with incorrectly installed prerequisites like RubyGems, running `bundle install` from the wrong place (Jekyll's ["Quickstart" page](https://jekyllrb.com/docs/) curiously elides this requirement), and an oblique reference on the [installation page](https://jekyllrb.com/docs/installation/#requirements/) to things called "development headers". My versions of Ruby, RubyGems, GCC, Make, and Bundler are more than sufficient, but in spite of my efforts, I never managed to figure out whether I have _all_ development headers as specified, or even which ones are relevant, or how to get them if I need them, or what they really even are. (If you don't know what development headers are, [this Stack Overflow question](https://stackoverflow.com/questions/39323317/what-exactly-are-development-headers) is as much help as I can give you. If you do, pretend you don't and try to figure it out for yourself. Go ahead.)

One common problem I've run into again and again and again in my quest to get a professional start in software engineering is that documentation is often poorly written: information-dense frequently enough, sure, but not in a way that is accessible to intelligent beginners in the process of developing proficiency. The writer just implicitly assumes that the reader doesn't require any context to understand everything. It's not a fun game to play.

We've all been there though; enough complaining. Funnily enough, `jekyll new .` without bumbling with `bundle` did have an effect. I was wary of trying it because GitHub's instructions imply that `bundle exec` should be used if Bundler is installed, and also because of a comment on Jekyll Talk that seemed to correlate leaving out the prefix with using Jekyll globally (which Jekyll warns against). But it did have an effect. It informed me of a conflict: my repository directory was not empty.

You may wonder why the necessity of this had not been clear to me.

[Step 1](https://jekyllrb.com/docs/step-by-step/01-setup/) of Jekyll's tutorial, for example, seemed to imply that a Git repository can be initialized asynchronously with the rest of the setup. Oh well. I was informed that it was possible to proceed by using a `--force` parameter.

Even after removing the Markdown file containing the intended post content, though, there would still be some Git configuration stuff for Jekyll to complain about. On the other hand, this seemed like a good example of when to start over, so in the interest of determining the Correct Way to proceed with the whole mess, I decided to completely eliminate the repository: `rm -rf .git` to erase the local history, then create the Jekyll site without any hangups, then force-push later to take care of the remote situation. Heavy-handed, yes, but who knows?

Jekyll initially declined to cooperate on the grounds that my account "isn't allowed to install to the system RubyGems"; `bundle install --path vendor/bundle` was the suggested solution (or `sudo`). The former worked fine, at the maddening cost of suddenly having the directory I'd prepared for use as my repository cluttered up with a bunch of opaque foreign objects—including, strangely, an unexpected `_posts` folder, put there not by Jekyll but by the `bundle install` command.

This seemed like an even better example of when to start over.

Surely I'd skipped a step somewhere or misunderstood a tacit bit of crucial context along the way. Was it just a universally understood fact that all of these files belonged in everyone's GitHub Pages repositories? Well, yes, yes, and yes. Branching out in my study of Jekyll's documentation, I returned to the [Ruby 101](https://jekyllrb.com/docs/ruby-101/) page and understood it a little better the second time around. In particular, it was at this point that I fully appreciated that Bundler _is a gem_, which was interesting, and that the `bundle install` command organizes the versioning and dependency requirements of all of the gems listed in the Gemfile, which in retrospect seems like a very reasonable and helpful thing for it to do.

And then something even more interesting happened: there's a link at the bottom of that page to another page about [Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/), in a "Tutorial" section of the site. Who knew there were tutorials? Not me, because they are only shown in the sidebar here, and not indexed on the front page or several other pages I had covered trying to put together what to do.

Can't help but feel that this might have been a useful resource to know about a little earlier. The steps and explanations were clearer than on the Quickstart and Installation pages, including a comment anticipating using the `--force` parameter and an explanation verifying that `Gemfile` and `Gemfile.lock` should be checked in to version control, and even a sample `.gitignore` indicating what doesn't need to be. Just one thing though. This very helpful tutorial also contains the following note:

>**This is not the simplest way to start using Jekyll**
>
>This tutorial helps you get Jekyll set up using Bundler, and optionally without any system-wide gem installations. If prefer [sic] installing the jekyll command to your default gem installation directory, you might want the [Quickstart](https://jekyllrb.com/docs/).

_"You might want."_ The Quickstart that I had been diligently attempting to follow long before I discovered this page all about installing Jekyll in the manner _strongly recommended_ by Jekyll and GitHub. Also, this seems to imply that the Quickstart instructions will result in system-wide gem installations, which I had wanted to avoid (and had been thinking I was on the right track to).

It was only at this point that I realized that there are also a few hours of [tutorial videos](https://jekyllrb.com/tutorials/video-walkthroughs/) available, courtesy of Giraffe Academy, that cover how to set up Jekyll—including resolutions to some of the sticking points I'd come up against. In retrospect, going to the modest effort of discovering them would have been time well spent in my case, particularly earlier in the process. (Note to self: find tutorials on YouTube, I guess. That wouldn't have occurred to me before, but as this whole ordeal vividly illustrates, I have a lot to learn.) At any rate, some more back-and-forth between these videos and the accompanying documentation was enough for me to finally figure out how to iron out enough details to get here.

### Don't follow in my footsteps

I am a big proponent of learning by doing. But as far as I can tell, I did practically everything out of order, and spent a bunch of time chasing domain-specific tidbits that probably aren't going to prove otherwise useful anytime soon. So instead, do this:

0. **Read everything first.** I'm not certain that the remaining steps will work perfectly, because I made a big mess of trying to figure them out. Alternatively (or better yet, in conjunction), check out the tutorial videos, though be advised that they are not free of erroneous details. If, after looking through everything, you feel that you have a better idea of how to proceed than the following, go for it. Otherwise,
1. **Create the directory** where you will keep your project files and navigate into it. Mine is called dkmccandless.github.io because that is the name GitHub Pages says it requires on its end and I like to think that I am keeping things simple.
2. **Install [Bundler](https://bundler.io/)** with `gem install bundler`.
3. **Follow the instructions** on [Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/) through "Create a Jekyll Scaffold". Notably, this will result in Jekyll being installed and a Jekyll site being created. Optionally, for warm fuzzies, go ahead with the `bundle exec jekyll serve` step too. Remember how we needed an `index.html` file? Surprise! (This was indeed a pleasant surprise for me; while I don't quite _hate_ puzzling through HTML, I wasn't exactly looking forward to it either.)

If you've already got Jekyll working, and you haven't slept yet since starting the whole process, then you're already way ahead of me. Now let's involve Git:

4. Jekyll helpfully provided me a `.gitignore` file, but it only dealt with files created by Jekyll. The sample under ["Commit to Source Control"](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/#commit-to-source-control) handles Bundler's stuff too, so **use it instead**.
5. **Create a repository** on GitHub called (user).github.io. Keep it empty; in particular, do not initialize it with a README.
6. Initialize a local repository in your project directory with `git init`. Contrary to popular belief as reflected in various tutorial videos, you don't need to use a branch named `gh-pages`.
7. Follow steps 8-10 under ["Creating your site"](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-your-site) to **edit your Gemfile**.
8. After uncommenting the `gem "github-pages"` line, it was necessary for me to run `bundle install` and then `bundle update` again to get all of the dependency requirements in order.
9. According to the Giraffe Academy tutorial video on integrating with GitHub Pages, you must change the `baseurl` variable in `_config.yml` to the name of your repository (prepended with a forward slash, which the video doesn't mention). However, in my site's case, it only worked when I didn't do that. At any rate, have a look inside `_config.yml`. If you've already come up with a snappy title and witty description for your blog, those go there.

That's the setup; you can push what you have to GitHub now, and if your publishing source is properly configured in the GitHub Pages section of your repository's settings, hopefully you'll have some skeleton content waiting for you at https://(user).github.io.

From here, there are a lot of customization options that are by definition outside the scope of this post. There is a default theme called Minima, and if you are reading this because you're a beginner who doesn't care how your blog looks, that should work for you.

### The easy part

As for actually blogging, that's the part whose simplicity is the main attraction of using GitHub Pages with Jekyll in the first place, and here at least, I'm happy to report that they don't disappoint:

10. Format your post as a Markdown file beginning with basic [front matter](https://jekyllrb.com/docs/front-matter/) between trios of dashes:
```
---
layout: post
title: Concurrency Beginners For
---
```
11. Name the file according to the [required format](https://jekyllrb.com/docs/posts/) (`yyyy-mm-dd-concurrency-beginners-for.md`), save it in the `_posts` directory, commit it, and push it to GitHub.

And that's all there is to it. Maybe.

My takeaways from this little journey are twofold:

* __Balance breadth- and depth-based learning__ in your approaches to new tools. While it's a big ask when organizing information from square one, in the long term it's also crucial for high-level comprehension. Rabbit holes are often fun and sometimes necessary, but make sure never to lose sight of the big picture, because...

* __Documentation is often buggy.__ The purpose of documenting code is to inform humans without forcing them to read the code itself. As the human-facing specification of a piece of code's behavior, the documentation's development should be held to the same standard of rigor as the code itself and, like the code itself, it should be diligently reviewed for accuracy, completeness, and clarity. The alternative runs the risk of hampering the ability of humans to effectively utilize the code for its intended purpose. The effectiveness of a piece of code is a function of its design and its usability; documentation can't help much with the former, but it is vital to the latter.