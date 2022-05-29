---
title: "Using Github Issues as a Jekyll frontend with Github Actions"
layout: post
categories: [Github]
tags: Github Jekyll
---
Recently I switched from WordPress to using Jekyll via Github pages. I quickly got into the habit of dumping blog post ideas into issues on my blog's repo. It's a great way to iterate on them and get feedback on them before actually publishing them.

![image](/assets/img/119626686-54434f80-bdc0-11eb-858d-9777e3148a8b.png)

The ideas keep accumulating, now how to get them published quickly? By automating it a much as possible!
While doing some research on how I could automate it via Github actions I came across this article:
[Using Github Issues as a Hugo frontend with Github Actions and Netlify](https://shazow.net/posts/github-issues-as-a-hugo-frontend)

Now this isn't exactly what I am looking to do, but it helped provide a base to start with. 
## Architecture
My blog is statically generated using [Jekyll](https://jekyllrb.com/), the [code is hosted on Github](https://github.com/locus313/locus313.github.io).

The blog post drafts are posted as Github issues, so how do we convert issues into pull requests? With Github Actions of course!

## Github Action: Issue to Pull Request
My [full workflow lives here](https://github.com/locus313/locus313.github.io/blob/master/.github/workflows/publish.yml) if you want to jump ahead, but let's break down the sections of the workflow.

I wanted to trigger the workflow once an issue is labeled with the 'publish' label, so let's start with that:

{% raw %}
```yaml
name: publish-post

on:
issues:
types: ['labeled']

jobs:
build:
if: ${{ github.event.label.name == 'publish' }}
runs-on: ubuntu-latest
steps:
...
```
{% endraw %}
Next up we want to specify the steps, first is to check out the repo into the action's environment:
```yaml
- uses: actions/checkout@v2
```
Once the source code is available, we want to generate the blog post from the issue metadata. Here is what I used:
```yaml
- name: Generate Post
env:
POST_TITLE: ${{ steps.title.outputs.title }}
POST_BODY: ${{ github.event.issue.body }}
run: |
preprocess_bin="$PWD/frontmatterify"
save_to="_posts"
cd "$save_to"
$preprocess_bin > "${{ steps.date.outputs.date }}-${POST_TITLE}.md" << EOF
$POST_BODY
EOF
```
This puts the body of the issue, which is already markdown, into a markdown file named based on the current date and title of the issue. This is a good place to add frontmatter, or slugify the title, or whatever else your blog setup requires.

And finally, we make the pull request using Peter Evan's create-pull-request action which makes this super easy:
```yaml
- name: Create Pull Request
uses: peter-evans/create-pull-request@v3
```
This is the minimum of what we need, but we can specify all kinds of additional options here: auto-deleting the branch, setting a custom title, body, and whatever else. Here's an example of what I'm doing:
```yaml
- name: Create Pull Request
uses: peter-evans/create-pull-request@v3
with:
delete-branch: true
title: "publish: ${{ github.event.issue.title}}"
body: |
Automagically sprouted for publishing.

Merging will publish to: https://lo5t.com/${{ steps.title.outputs.title }}

Closes #${{ github.event.issue.number }}
reviewers: "${{ github.repository_owner }}"
commit-message: "post: ${{ github.event.issue.title }}"
```
## Result
When my blog post draft is ready, I add the publish tag and the Github action starts the workflow, creating a pull request:

![image](/assets/img/119776493-de9bba00-be79-11eb-9bb5-e213cd7da9b8.png)

The pull request automatically pings me as a reviewer and includes a "Closes #X" line which will close the draft issue once the PR is merged. Very convenient!

![image](/assets/img/119776748-2cb0bd80-be7a-11eb-9b76-e8904cafb8ac.png)

I can make sure everything looks good, and even apply edits directly inside the pull request. This is another great step to send a long blog post for feedback, using all of the wonderful Pull Request Review features!

When all is said and done, merging the pull request triggers Github pages to publish the changes, and merging closes the original issue, and I'm done!

## Bonus
Drag n' drop images work in Github Issues, so it's super easy to write a quick post with a bunch of screenshots.

However I do want to make sure I have local copies of the images, so the workflow makes sure that all of the published images are downloaded to my Github repo.

I can still make blog posts by pulling the latest repo, write some markdown, and push to publish.

I added the [frontmatterify script](https://github.com/locus313/locus313.github.io/blob/master/frontmatterify) that was originally created by [Andrey Petrov](https://shazow.net/) to process the incoming markdown and convert the remote Github Issue uploaded images into local images that are included in the pull request. The script also generates frontmatter that is used by Jekyll. It required a little tweaking to get it working for my use case.

Now to start publishing.
