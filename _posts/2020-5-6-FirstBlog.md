---
layout: post
title: GSoC'20 selection - My journey with SymPy
---

Yesterday, on May 5, 2020, UST 18:00, Google announced the results for the GSoC'20 program. My hard work of over three and a half months finally paid off and I was selected for the program.

I am glad that I applied for SymPy since its not only one of the best organizations to work within my opinion based on how well the community functions but also because I  love mathematics and programming in Python, and SymPy brings both of them together. It is fascinating to me that I am creating a piece of software that can solve mathematical equations.

My journey with SymPy started when I was looking for organizations to work within the GSoC program since I had thought then that I would love to learn more about how open source functions regardless of my selection. I was browsing through organizations that were primarily using Python given my 2 years of experience with the programming language. When I was browsing through organizations, I would normally go for their documentation straight and set up their library code using ```git clone```, then I would read through the documentation and try to follow steps I learned in my local machine. This gave me a grasp of the library and allowed me to be more clear of whether I would want to work for this org or not. I tried this for a few organizations before I found SymPy. When I was learning for SymPy, I was amazed by the fact that the documentation was so clear that sometimes, glancing just at the example gave the essence of what the function or class is all about. Then, I decided I would love to see more about how this organization functions as a community. For some days, I would just observe how different people of the community worked together on various platforms, be it the Gitter channel or in issues/PRs. I didn't introduce myself yet as I was still looking for ways to introduce myself.

# Understanding the organization

While I was looking for ways to work with the community by merely observing different members of the community, to my amazement, I found out that SymPy had already made a page about [Introduction to the contributing](https://github.com/sympy/sympy/wiki/introduction-to-contributing). I found this out when someone introduced themselves to the gitter channel and one of the members replied with this link. I went through this page multiple times to avoid any big unnecessary errors on my side. I went through many links mentioned on the page like:
1. [SymPy Tutorial](http://docs.sympy.org/latest/tutorial/): Here, SymPy gives a tutorial on how to use the library. This is not only for the contributors but generally for the end-users or someone who has zero experience with the library. I would recommend anyone who has an interest in joining SymPy organization(or any for that matter) to go through the tutorials as it not only provides you an understanding of the library but it also gives you an idea of what an end-user expects from the organization.
2. [SymPy’s Architecture](https://docs.sympy.org/latest/guide.html#sympy-s-architecture): This section is important for people who want to become contributors as it gives necessary information about how the code is organized and since a typical contributor has to work with code from multiple parts for a single update or task, knowing this would come in handy.
3. [SymPy’s documentation](https://docs.sympy.org/latest/index.html): Now, when it comes to documentation, reading it completely may not be necessary as the initial steps since the reading documentation completely is a big task, but it is advised to at the very least glance at it. But, when a contributor has taken up a task or an update, then it is a good idea for them to understand the module they are working with by going through the documentation of that module(fully or partially, as required). The link provided is an index and there are links to many things like docs of different modules, installation guides, etc but among them, all, [SymPy Documentation Style Guide](https://docs.sympy.org/latest/documentation-style-guide.html) needs to be thoroughly read by a new contributor as documentation is a vital component of a healthy open source library and following the style guide ensures that the documentation stays in a good format so that it is easy for any end-user to pick up how to use the part of the code or the functionality added by a contributor.
4. [Code of Conduct](https://github.com/sympy/sympy/blob/master/CODE_OF_CONDUCT.md): New contributors should rememberthis  when interacting with the community.
5. [SymPy's mailing list](https://groups.google.com/forum/#!forum/sympy): I saw through the mailing list and learned a lot about different discussions that happened in the community and how members helped new contributors(be they GSoC aspirants or just people who are willing to learn more about the library). I saw through mails in the group especially the ones where members told the newcomers what they need to do to become a contributor. I saw the links that the members shared and if it wasn't one of the links that I have already gone through, then I would give time to read them.
6. [Development workflow](https://github.com/sympy/sympy/wiki/Development-workflow): This is one of the most important pages I went through before becoming a contributor. I gave it a read initially and made all the necessary initial setup if some of the steps mentioned in the workflow weren't done already. I later came back to this link again and again when I made my first contribution as I didn't want to make any big mistakes from my side. I adhered to the commit message style mentioned as a detailed commit is necessary for maintainers to understand the code by contributors. They have the task of merging the code into the codebase and they need to understand completely what a contributor has done in his/her update.

These pages helped me grasp how the community functions as a whole and how I as a new contributor, should work with the community to efficiently contribute to the library.

# First Contribution

> A journey of thousand miles begins with a single step

After gathering all the information that I can about the community and the library, I finally introduced myself to the gitter channel. I didn't ask for any tips as I had already seen numerous tips given to my fellow new contributors and I knew that now, I had to start with contributing right away. I searched for **easy to fix** issues in the library and started working on one of them. [Issue #18412](https://github.com/sympy/sympy/pull/18416) is the first issue that I worked for. Back then, I was a novice when it came to contributing even after reading all the details of contributing. But, [Oscar Benjamin](https://github.com/oscarbenjamin) helped me and answered my queries. Then, I made a separate branch in my local SympPy's repo, made the necessary changes, and pushed the new branch to my forked repo of SymPy. Before pushing the changes, I made sure that all the required test cases passed and that the code quality was good. After the testing, I pushed the changes.

With GitHub's features, I was able to easily create a PR from the branch and I filled all the necessary details in a template like a format to make a new PR. I mentioned the issue that this PR was fixing, added description, examples, and release notes in the format mentioned. 

Finally, after everything was done, my PR was merged and I was happy that all the prior preparation was helpful in the end. 

Note: Throughout the process of this first contribution, I adhered to all the rules that the community expects the  contributors to follow and re-read the [Development workflow](https://github.com/sympy/sympy/wiki/Development-workflow) since this page had all the details of creating a PR, testing, commit message style and making changes.

# Other contributions

After my first contribution, I made around 18 PRs till today. I was late compared to my other fellow GSoC aspirants. And some of them had many contributions already when I was just learning the fundamentals of the library. So, I made sure that I have a bare minimum number of PRs before making my GSoC proposal since I already knew based on past selections that one of the key factors that SymPy weighed heavily on is the contributions. I decided the bare minimum to be 15 PRs. 

So, I worked on 14 other PRs which involved either closing **easy to fix** issues, issues from a specific module that was easy to deal with or, reviving and finishing the PRs that needed to be revived. The last one required more skill to get done and it also gave me an essence of how to work with someone else's code along with upgrading my coding skills. I tried to learn from other good contributors code who have become inactive and it gave me a lot of insight on how to work with the SymPy's library code and how to make new functionality.

I learned a great many things like how to revive a PR wherein the previous author's contributions are visible, how to rebase a git repo and squash commits, different Python and SymPy features, etc and this was all thanks to the reviewers such as  Oscar Benjamin(oscarbenjamin), Aaron Meurer(asmeurer), Gagandeep Singh(czgdp1807), Kalevi Suominen(jksuom), S. Y. Lee(sylee957) and Christopher Smith(smichr) who gave me necessary reviews and tips wherever I needed. 

Two of the revived PRs aren't completed yet but I was able to effectively contribute to 13 PRs that got merged. One of the revived PRs is in YAGNI(You Are not Gonna Need It) status. 

# Project Decision

I was very new to the community even after a month of contributing and the proposal dates were closing in. I had no choice but to look from the [GSoC'20 ideas](https://github.com/sympy/sympy/wiki/GSoC-2020-Ideas) page by SymPy since I didn't have time to come up with my own idea. Some of my friends have already got selected in past GSoC programs(one of them in SymPy) and they suggested the same. So, I went through the ideas page and found the project related to solving systems of ODEs using matrix exponentials. 

Many of my fellow GSoC aspirants had already introduced themselves in the mailing list and they had already showcased their interests in one of the projects from the ideas page according to their liking and contributions. Some of them had started working on it already. So, to avoid unnecessary competition, I decided to choose a project that no one had selected up until that point. Since, the systems of ODEs solver project wasn't chosen by anyone and along with that, [Oscar Benjamin](https://github.com/oscarbenjamin) had already made a very great and detailed [roadmap](https://github.com/sympy/sympy/wiki/ODE-Systems-roadmap), I decided that I would work with this project. I introduced myself to the mailing list and after some discussions, I finalized my choice for this project. 

# Proposal making and new PR 

Now, I started reading through the roadmap and I knew that I had to learn the theory involved first. I had to learn it in detail before even making a proposal and contributing regarding the same. So, I started learning about the ODEs from various books that I got from my college library. I had an experience of solving ODEs, but before this, I never paid much attention to the theory and just used the techniques that I was taught. This time I made sure I understood the nomenclature used to describe the system of ODEs, and various techniques of solving them, even the advanced ones. 

After learning enough details about ODEs and systems of ODEs, I came across matrix exponential which is used to solve some cases of systems of ODEs. I learned about matrix exponentials and all the necessary concepts like Jordan blocks, Jordan matrices, etc by going through Wikipedia articles. But, going through it all took some time as I wasn't aware of Jordan blocks and other concepts. Along with that, I was unclear about how it helped to get matrix exponential quickly. I spent some days trying to figure that out and finally, I did. I even mentioned it in my proposal. 

After learning enough about all the prerequisites mentioned in the roadmap, I started going through it. I read it several times since I didn't want to leave any stone unturned. Finally, I revived the PR involving matrix exponential and started working on it simultaneously. This PR was the biggest PR update that I had done until now so much so that it took more than a month to finish the work. But, the result was great and it helped the library a lot. 

Now, after discussions in the mail, where I gave a proposed timeline of my work for this summer, I was instructed to make a proposal that covered what the roadmap didn't, that is, how different parts introduced in the roadmap like solvers and functionalities would function together. I made the design which took some days, and I proposed it openly in the mailing list. I was instructed minor changes and after which, I started making my proposal.

For proposal designing, I went through numerous past proposals accepted by SymPy to get a grasp of proposal writing as I was a novice in the same. After going through various proposals, I made a structure of my own in pen and paper first. I went from top to down approach wherein I first designed the higher-level look of the proposal and soon focused on individual sections and subsections. After deciding the proposal structure and the content of the proposal, I began writing it in google docs. I was lucky that there was something called add ons feature in google docs which allowed me to render latex equations and show code in a pretty format. This made the look of the key parts of my proposal great. I learned a lot of the features in google docs and used them extensively to make a good proposal.

I submitted my proposal late compared to my fellow GSoC aspirants but I was able to complete the proposal before the deadline. 

# Waiting for the results

From April 1 onwards, I worked on my open PRs, especially the one which was important for my GSoC project. I was able to get two of my open PRs merged in this month but one of them was the toughest one I dealt with.

# Selection

I was eagerly waiting for the results and refreshing the tabs of my GSoC dashboard. At exactly UTC 18:00, the results came and I was overwhelmed with joy. The two reasons for which I am selected are probably my consistency as I was working almost every day for the org in some way or another, and learning new things along with incorporating it into my work. 


