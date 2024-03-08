---
layout: post
title: TIL - Auto-assign an issue on GitHub 🤖
---

I like the power of a nice simple [GitHub action](https://docs.github.com/en/actions) to automate a specific activity in a repo. For example, I recently shared a repo ([feeds](https://github.com/ajmaradiaga/feeds/)) where I "publish" various custom RSS feeds based on data from the SAP Community. In my case, I want to be automatically assigned to any new issue created in that repo, so I get notified whenever there is a new feed request (via an issue). I had some new requests (issues) open for a few days and it was only because I wasn't aware of them. That's why I need to automate it and not have the need to check the repo for new issues. For this, I'm using the Auto-assign issue action available in the GitHub action marketplace - [https://github.com/marketplace/actions/auto-assign-issue](https://github.com/marketplace/actions/auto-assign-issue).

> You can find more details regarding the SAP Community feeds here: [Exploring the RSS feeds of the new SAP Community platform](https://ajmaradiaga.com/RSS-feeds-in-SAP-Community/) and [Additional SAP Community feeds 📡](https://ajmaradiaga.com/additional-SAP-Community-feeds/).

Below is the GitHub action that I added to the [feeds repo](https://github.com/ajmaradiaga/feeds/blob/main/.github/workflows/autoassign.yml).
```
name: Issue assignment
on:
  issues:
    types: [opened, unassigned]
jobs:
  auto-assign:
    runs-on: ubuntu-latest
    permissions:
      issues: write
    steps:
      - name: "Auto-assign issue"
        uses: pozil/auto-assign-issue@v1
        with:
          assignees: your_username
          numOfAssignee: 1
          allowSelfAssign: true
```
