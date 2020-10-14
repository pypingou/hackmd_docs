# GitLab AMA session with Fedora

- Date: September 10th 2020 
- Time: 13:30 UTC
- Location: #fedora-meeting-1 on irc.freenode.net 

---

## How to use this document?


### Adding a question:

Copy the following section at the **bottom** of the page and add your question as want.

````
- Question:
- Votes: 
- Answer:
---
````

### Voting on a question:

Simply add a ``+1`` on the line saying ``Votes:``. Do **not** edit previous votes, do **not** add to the existing count, just add your own ``+1``.

For example, a question with three votes should look like:

````
- Votes: +1 +1 +1
````

On September 9th, the questions will be ordered here by their popularity, so the most popular questions are asked first.


### The answer section

The answer section is for **GitLab** to provide an answer to the question. **Please do not fill it for them**. All of the Q&As will be published after the session.

---

## Questions

- Question: Fedora has a group-based access system. People in the `packager` group have (commit) access to only the packages they maintain. People in the `provenpackager` group have (commit) access to all the active packages, but a few (for legal reason). People in the `releng` group have commit access to all the packages. Is this an access model that GitLab can support? If not, how would this work in a GitLab world? How would notifications work (Esp consider people in the `provenpackager` or `releng` group do not want to be notified for all the projects they have access to)?
- Votes: +1 +1 +1 +1 +1 +1 +1 +1
- Question: Can gitlab prevent people from being added to a project if they are not member of a group? (in Fedora's case the `packager` group)
- Votes: +1 +1 +1
- Question: A follow to above question, Fedora has 3 levels of access for each repo, `owner`, `admin` and `commit`. There will be a single owner with multiple people with admin and commit permissions. This gives granular level of access on the repos. Is this possible in GitLab?
- Votes: 
- Answer:

[cverna]
What I explored was something along the lines of : 
Packager → Using GitLab’s Maintainer or Developer role for the project they maintain (Maintainer have the ability to access project settings and change pretty much everything there, so that might be blocking here, Developer only have commit access, so we need another way to change some settings for Packagers)
Co-Maintainer → Using GitLab’s Developer role (commit access)
Proven-Packager → GitLab’s Developer role on all repo (expect 2)
Release Engineer/Admin/etc .. → GitLab’s Owner role on all repos 

This is not an exact matching with what we currently have but should give us a way to experiment with this and look at what is acceptable or not.
There is also a [GitLab ticket](https://gitlab.com/gitlab-org/gitlab/-/issues/7626) to implement policies for the project that could give more granular control of permissions.
Gitlab’s notifications are quite granular and can be managed at the different levels (Merge Requests, Projects, Group, Global) https://docs.gitlab.com/ee/user/profile/notifications.html#global-notification-settings

---

- Question: Fedora uses a message bus to integrate different parts of its infrastructure. How should we onboard GitLab into this message bus?
- Votes: +1 +1 +1 +1 +1 +1 +1 +1 +1
- Answer:

[cverna] Currently we would need to have a service that proxies GitLab’s events to fedora-messaging something similar to github2fedmsg. 
There were some concerns raised about the order of events sent by GitLab’s webhooks, these will need to be looked after during a Proof of Concept stage.

---

- Question: Fedora forbids force-push in the main projects but allows them in forks. Would this be feasible in GitLab in a way that people cannot revert?
- Votes: +1 +1 +1 +1 +1
- Answer:

[cverna] This can be done with [Protected branches](https://docs.gitlab.com/ee/user/project/protected_branches.html#protected-branches)

---

- Question: could gitlab (inc) maintain a Community Edition GitLab instance that Fedora uses?
- Votes: +1
- Answer: 

There is no plan to create custom versions of GitLab for customers. Instead, GitLab encourages paid customers and free users alike to contribute upstream to make sure that GitLab continues to work well for the most amount of users possible. As an open core company, GitLab has a [public roadmap](https://about.gitlab.com/direction/) and works with its community members to build a great product.

GitLab regularly engages with its community and takes into account its feedback. As a result, features are often ported down into lower tiers in order to make the Community Edition and Free tiers continuously more useful (see example of [18 features moved to open source](https://about.gitlab.com/blog/2020/03/30/new-features-to-core/)). 

[greg] GitLab hosting is available to users of GitLab.com SaaS, but GitLab does not offer hosting and management for GitLab CE or EE instances.

---


- Question: Fedora supports the concept of retired `project` (ie: archived) that no-one can commit to. Does GitLab have an equivalent concept? (The retired status is not something project admins can change)
- Votes: +1 +1 +1 +1
- Answer:

There is the possibility to [archive projects](https://docs.gitlab.com/ee/user/project/settings/#archiving-a-project)

---

- Question: Fedora supports the concept of retired git `branch` (ie: archived) that no-one can commit. Does GitLab have an equivalent concept? (The retired status is not something project admins can change)
- Votes: +1 +1 +1
- Answer:

[cverna] Yes this is possible with [Protected Branches](https://docs.gitlab.com/ee/user/project/protected_branches.html#protected-branches)

---


- Question: Branch level permissions? Can we set the above permissions at branch level? Esp can we set them with some regex matching?
- Votes: +1 +1
- Answer: 

[tfigueiro] Branch permissions accept wildcards, but not regexes. Regex can be used with custom push rules. See https://docs.gitlab.com/ee/push_rules/push_rules.html and https://docs.gitlab.com/ee/user/project/protected_branches.html#configuring-protected-branches. 

---


- Question: Extending to above question, the sla's or eol dates are coming from a 3rd party service, when a user pushes to git branch, it will check the eol in that service and rejects it if it is already eol'd. Is this possible in GitLab?
- Votes: +1 +1 +1 +1
- Answer:

[cverna] Instead of this we would probably use the Protected Branches in GitLab. We could leverage the APIs to mass configure the branches on eol dates. That would mean that for each git push we don’t need to look at a 3rd party service to know if we can push or not.

---

- Question: Is it possible to hide (or de-emphasize) these retired branches from the UI? Say, hide branch names with f30 or lower?
- Votes: +1 +1
- Answer:

[BrendanOLeary]: This is not possible currently
---


- Question: In addition, the above restriction is hardcoded, so even a dist-git administrator can't revert that. This is very useful to avoid cases like https://pagure.io/fedora-infrastructure/issue/9227 . Would be feasible in GitLab to achieve this restriction to gitlab admins?
- Votes: +1 +1 +1 +1
- Answer: 

[cverna] - I am not sure :-)

---

- Question: Fedora supports `+` in repo name, there is a [ticket](https://gitlab.com/gitlab-org/gitlab/-/issues/220309) on it, but it seems to be closed with status being tracked in a private ticket. What is the status on it?
- Votes: +1 +1 +1 +1 +1 +1
- Answer:

issue https://gitlab.com/gitlab-org/gitlab/-/issues/220528 has been made public and can be used to track support for `+` in project names.

---

- Question: What is the bandwidth availability? We need to create branches and commit on the main branch of all the 30k+ repos during certain times of the development cycle. Is that possible and how efficient is it?
- Votes: +1 +1
- Anwer:

[cverna] This highly depends on where the service is hosted, but generally there is some API rate limiting which can be configured per instance. So we should be able to find a sweet spot there.

[greg] Bandwidth in terms speed and availability is only limited by the network speed of the environment where GitLab is hosted. (10Gbps > 1Gbps > 100Mbps). Outside of network speed  limitations, having [hardware](https://docs.gitlab.com/ee/install/requirements.html) resources can help ensure performance and availability at scale.


---

- Question: Integration with 3rd party tooling? In current dist-git, we can look at the status of the package like whats the latest build in each release, koschei, bz and other links. Orphaned packages can be adopted based on certain rule set. Can we achieve this in GitLab?
- Votes: +1 +1 +1 +1 +1
- Answer:

[cverna] Links to other services can be done by using GitLab’s badges (https://docs.gitlab.com/ee/user/project/badges.html). There is an open ticket which would make it easier to configure at group level (https://gitlab.com/gitlab-org/gitlab/-/issues/22278)
For UI integration (orphaned packages, etc …) we will not be able to have the same level of integration as we currently have with Pagure.

---

- Question: It is not allowed for users to delete the branches, only certain people with certain access levels are allowed to perform this operation. Is this possible in GitLab?
- Votes: +1 +1 +1
- Answer:

[cverna] Yes this is possible with [Protected Branches](https://docs.gitlab.com/ee/user/project/protected_branches.html#protected-branches)

---

- Question: A user is not allowed to rewrite the history. Is it possible? It would be nice to allow only certain people to do so. Although we might not use this, but its good to have just in case if we ever need it.
- Votes: -1 +1
- Answer:

 [tfigueiro] by default, only the `master` branch is protected against force push and deletion. It’s possible to configure other branches to be protected and who has permission to override. https://docs.gitlab.com/ee/user/project/protected_branches.html#configuring-protected-branches 
 
---

- Question: Orphaning a package or repo? A maintainer (owner) of the package can decide to orphan a package, the maintainer should only be able to assign the package to the `orphan` user or any of the other maintainers of the package only. Is this possible to set this branch level as well?
- Votes: +1 +1
- Answer:

[Brendan, GitLab]: Yes, in the sense that who is allowed to push or merge to a specific branch is controllable at the user or group level. This can be at the specific branch level or based on a wildcard match based on branch name. See [configuring protected branches](https://docs.gitlab.com/ee/user/project/protected_branches.html#configuring-protected-branches) for more details.

---

- Question: Will this lead to better support for Podman in CI workflows in GitLab?
- Votes: 
- Answer:

[greg] Short term solution might be using a [custom executor](https://docs.gitlab.com/runner/executors/custom.html), long term solution would be getting the [Runner executor podman (#4185)](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/4185) feature request issue scheduled and closed. Ultimately [product team schedules work](https://about.gitlab.com/handbook/product/product-processes/#how-we-prioritize-work), while everyone can contribute MRs or fixes ahead of schedule. In the past, I've seen a lot of enthusiasm from GitLab team members in helping solve problems from Open Source Program members whenever possible.

---

- Question: Will some development effort from Fedora also be put into GitLab?
- Votes: +
- Answer:
---

- Question: Currently dist-git in Fedora has several namespaces: rpms, modules, containers, tests... All namespaces but the ``tests`` namespace have their issue tracker in bugzilla. Would this work in gitlab? Can we selectively enable/disable issue tracking per namespace for the entire instance? (ie: w/o giving the possibility to ``owner`` or ``maintainer`` to toggle that setting.)
- Votes: +1 +1 +1 +1
- Answer:

 [cverna] - AFAIK this can be done per project, owner and maintainer can change this setting but owner and maintainers should only be sysadmin-main and releng.
Namespaces map to “group” in GitLab. Here’s more info about them: https://docs.gitlab.com/ee/api/namespaces.html 


[BrendanGitLab]: You CAN turn the GitLab issue tracker on and off by project. See https://docs.gitlab.com/ee/user/project/settings/#sharing-and-permissions 

---

- Question: Can project creation be restricted to a specific group of people in GitLab?
- Votes: +1 +1 +1
- Answer:

[cverna] Yes this can be configured at the instance level (https://gitlab.com/help/user/admin_area/settings/visibility_and_access_controls.md#default-project-creation-protection) or at the group level (https://gitlab.com/help/user/group/index.md#default-project-creation-level)

---

- Question: Can project (main project, not fork) deletion be restricted to a specific group of people in GitLab? (ie: project owner/maintainer must not be allowed to delete a main project, they can delete their own fork of course)
- Votes: +1 +1 +1 +1
- Answer:
---

- Question: Fedora, as far as I understand, still plan on using bugzilla as issue tracker. Currently default assignee and the CC are gathered using the ``main admin`` (ie: the ``owner`` for GitLab iiuc), the other maintainers (who did not ``unwatch issues`` in the project - mechanism for them to opt-out of being in the CC list) and the people having enabled ``Issue watching`` for the project (mechanism for them to opt-in into being in the CC list). Would this work in a GitLab world?
- Votes: +1 +1 +1
- Answer:

[Brendan] There are a number of options related to that.  For one, users can control their notifications globally and by name space in a fine grained way (see GitLab Notification Emails). 

---

- Question: Related to the previous question about bugzilla sync, Fedora's dist-git has grown the possibility to store an "override" of the main admin so a different person/group is set as the default assignee in bugzilla. How could we store that information in GitLab?
- Votes: +1 +1 +1 +1
- Answer:

[cverna] Not sure yet

---

- Question: In Fedora's dist-git, namespaces (rpms, modules, containers, tests) are not linked to groups. How would this work in a GitLab world?
- Votes: +1 +1 +1
- Answer:
---

- Question: To gain commit access on dist-git, a person needs to be added to the packager group. How would this work with GitLab?
- Votes: +1
- Answer:
---

- Question: How would group membership be sync to GitLab?
- Votes: +1 +1
- Answer:

[cverna] - I am not 100% clear on that, since GitLab supports OpenIDC, I am not sure if we could make use of the group scope returned by AAA. Otherwise we will need a solution to sync the groups to GitLab most likely using API calls.

---

- Question: In CentOS, members of a group (say: storage-sig) can push to any branch starting with `c<int>-<sig-name>-*` (so for example, they can push to : c8-storage-sig-v2.0 or c7-storage-sig), but they cannot push to any other branches, like: c8, c7 or any of the branches starting with c8-kernel-sig and so on. Would this be doable in GitLab? (in a way that project owner/maintainer cannot edit?)
- Votes: +1 +1
- Answer:


[greg] A [Custom Push Rule](https://docs.gitlab.com/ee/push_rules/push_rules.html#custom-push-rules) and [server hook](https://docs.gitlab.com/ee/administration/server_hooks.html) (pre-recieve Git hooks behind the scenes) could help with this.


---

- Question: How would git push over http work with GitLab? (considering gitlab does not have Fedora's password since they are stored in FAS)
- Votes: +1 +1 +1 +1
- Answer:

[greg] GitLab has a good number of authentication options and integrations where the "best" solution usually depends on a team's specific needs and use case. As such, I think the best way to know and meet your needs and use case is to have a conversation and discuss the options. How does git push over HTTP work with FAS right now, and what git push (over HTTP) auth requirements/flow would you like to have for your projects in GitLab?


---

- Question: Fedora policy restricts dist-git ssh push access to packager group, the rest of users must use http push. Would this be doable in GitLab? (in a way that project owner/maintainer cannot edit?)
- Votes: +1 +1 +1
- Answer:
---

- Question: All this ACL restrictions could change any time if we change our policies. Would be feasible to adapt all those restrictions to align with new policies?
- Votes: +1 +1
- Answer:
---

- Question: on Fedora, during beta and final freeze, the key infraestructure pieces are freezed too and any infra change that affects a key service is subject to review and aproval from releng/sysadmin-mainers. dist-git is one of those key services. In case GitLab inc. hosts a CE instance for Fedora, would GitLab respect this policy and stop changing anything on those boxes without previous aproval during those time frames?
- Votes: +1
- Answer:
---

- Question: Would GitLab respect our Changes Policy (https://docs.fedoraproject.org/en-US/program_management/changes_policy/) for major changes on dist-git instance?. Specifically, prior to migrating to gitlab, would we have a detailed change proposal that covers at least all the concerns that people wrote on this AMA and the related devel@ threads so we can discuss with a real, final proposal full of tech details before accepting this migration process?
- Votes: +1 +1
- Answer:

[cverna] - yes there was a FESCO ticket for that https://pagure.io/fesco/issue/2383

---

- Question: Is this process subject to discussion and aproval by Fedora? The related forum post says that Fedora made a decission, but that's not correct. And there are too much unanswered questions right now for a Fedora decission according to the Changes Policy. So, when/how will we discuss and accept (or not) this on a proper fedorian way?
- Votes: +1
- Answer:

[cverna] - yes there was a FESCO ticket for that https://pagure.io/fesco/issue/2383

---

- Question: The pagure instance for src.fedoraproject.org supports CI for some use cases, e.g. `simple-koji-ci` or `zuul` CI for pull requests against projects in the `rpms` namespace. Will this still be supported with GitLab?
- Votes:  +1
- Answer:
---

- Question: The pagure instance for src.fedoraproject.org supports use cases on top of "just" git hosting and basic project management, e.g. setting default BugZilla assignee overrides, setting anitya / release-monitoring.org status, etc. Can this be implemented on top of GitLab, or will a separate service need to be implemented for this (something like pkgdb)?
- Votes: +1 +1
- Answer:
---

- Question: We were told in March that only dist-git (src.fp.o) was going to move to gitlab. Could we get confirmation on this as https://forum.gitlab.com/t/fedora-migration-to-gitlab-ask-me-anything-ama-thursday-september-10-2020/41971/2 seems to imply otherwise
- Votes: +1
- Answer:

Compliance is an ongoing process and we work diligently to keep up with best practices and processes every day. GitLab has a [GitLab Privacy Policy](https://about.gitlab.com/privacy/) to [Protect your Data](https://about.gitlab.com/privacy/#protecting-data), including internal [Information Security Policies](https://about.gitlab.com/handbook/engineering/security/#information-security-policies). If self-hosting GitLab, you ultimately control your data and where it lives.

---

- Question: In case we move to a GitLab (inc) hosted instance, and after reading https://lists.fedoraproject.org/archives/list/devel-announce@lists.fedoraproject.org/thread/Q74QYG54MNBCY7UX2GITPAZYKHD6BYFH/ , what's the plan around GDPR/CCPA ? Would we lose all historic data due to that? (esp considering Fedora contributors may not agree to share their PI to gitlab)
- Votes: +1 +1
- Answer:

**********************************

[Aoife] Please note: below this line any questions that are added may not be answered during the session. I am sending the above to GitLab now and will publish their answers after the session. Thanks everyone for contributing some really fantastic questions and I'm looking forward to the AMA session tomorrow! :)