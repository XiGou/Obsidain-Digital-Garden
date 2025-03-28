---
layout: post
title: "[Literature Reading] Modern Code Review: A Case Study at Google"
date: 2021-12-14 21:35:04
tags: [Software Engineering]
authors: [xigou]
---

Recently, after called some code review meetings in my daily work, I got interested about code review practice, so I read this excellent article on the weekend. 

<!-- more -->
<!--truncate-->

## Hisroty of code review practice

1. Code Inspections

It's a very  structured process style code review, when developers finished code, they shoud working on planning, overview, preparation, inspection meeting, reworking, and follow-up for code review, reviewers and code authors will sitting in the same room, and they can talk with the author face to face to understand the code and find the defects.

2. Asynchronous review via email

Until the late 2000s, most large OSS projects adopted a form of remote, asynchronous reviews, relying on patches sent to communication channels such as mailing lists and issue tracking systems. So this method broke the limitations of "sitting toghter" and authors don't need to do a presentation to explain the code, instead they should explain the code in the length of one email.

3. Tool-based review

The main-streamd code review practice nowdays is tool-based, the representative of review systems are Gerrit, CodeFlow, ReviewBoard and Phabricator.

*Typical workflow of tool-based review*:
    
    1. author submit code patch to review system,
    2. reviewers review code and post comment on specific lines of code,
    3. authors reply comments or submit new patch to review system,
    4. repeat step 2-3 until all reviewers or specific role of your team think the patch is ready for testing or merging.

4. Pull-based development model

Mainly used in Github or similar code repo website, very similar to Tool-based review.

## This research answered three Research Questions about code review:

### RQ1: What are the motivations for code review at Google?

> Finding 1. Expectations for code review at Google do not center around problem solving. Reviewing was introduced at Google to ensure code readability and maintainability. Today’s developers also perceive this educational aspect, in addition to maintaining norms, tracking history, gatekeeping, and accident prevention. Defect finding is welcomed but not the only focus.

This is in line with my daily experience, when fixing bug it very useful to search for a similar bugfix patch in review system, and the review system contains some auto process to do static check and testing.

Besides, The most important aim in business view is to grab out the knowledge out of the brain of the developer and make others easy to learn it, this makes every piece of code at least read by 2-3 employee inside organization, when some empolyee leave the organization, negative impact will be small.

### RQ2: What is the practice of code review at Google?

> Finding 3. The Google code review process is aligned with the convergent practice of being lightweight and flexible. In contrast to other studied systems, however, ownership and readability are explicit and play a key role. The review tool includes reviewer recommendation and code analysis result

The most impressive part for me is: In Google, every code directory has owner(a set of people), when you change the code of that directory, owner should review and approve the code. 

## Something interesting

> At Google, over 35% of  
the changes under consideration modify only a single file and  
about 90% modify fewer than 10 files. Over 10% of changes  
modify only a single line of code, and the median number of  
lines modified is 24. The median change size is significantly  
lower than reported by Rigby and Bird for companies such as  
AMD (44 lines), Lucent (263 lines), and Bing, Office and SQL  
Server at Microsoft (somewhere between those boundaries),  
but in line for change sizes in open source projects.

Even in big and fast-developing company like Google, most of the code modification is "small", when you usually need to change hundreds lines of code, that means something wrong with your company, please start to post your resume as soon as possible. 

## Write at the end

In my team now, I'd rather define the code review process as "Tool-based Code Inspections", we are using Gerrit to track code patch for review, but almost nobody will post comments or reply on it,  the reviewer have no interest to read others' code, developer should call a meeting and do a presentation to explain the code to reviewers, then reviewers will propose some comments in the meeting. 

# Refs
<div id="refer-anchor-1"></div>
- [1] Caitlin Sadowski, Emma Söderberg, Luke Church, Michal Sipko and Alberto Bacchelli. 2018. Modern Code Review: A Case Study at Google. In Proceedings of 40th International Conference on Software Engineering: Software Engineering in Practice Track, Gothenburg, Sweden, May 27-June 3, 2018 (ICSE-SEIP ’18), 10 pages. DOI: 10.1145/3183519.3183525

