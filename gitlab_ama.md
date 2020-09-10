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
- Answer: GitLab has groups and subgroups, more information at https://gitlab.com/help/user/permissions and https://gitlab.com/help/user/group/subgroups/index.md#membership - this only gives a partial answer to the question though.
---

- Question: Can gitlab prevent people from being added to a project if they are not member of a group? (in Fedora's case the `packager` group)
- Votes: +1 +1 +1
- Answer:
---

- Question: A follow to above question, Fedora has 3 levels of access for each repo, `owner`, `admin` and `commit`. There will be a single owner with multiple people with admin and commit permissions. This gives granular level of access on the repos. Is this possible in GitLab?
- Votes: 
- Answer: GitLab offers several access levels, `owner` (similar to `owner` in Fedora),`maintainer` (similar to `admin` in Fedora),`developer` (similar to `commit` in Fedora),`reporter` and `guest`.  An overview of permissions is available at https://gitlab.com/help/user/permissions - there is also a link to future roadmap on this page
---

- Question: Branch level permissions? Can we set the above permissions at branch level? Esp can we set them with some regex matching?
- Votes: +1 +1
- Answer:
---

- Question: could gitlab (inc) maintain a Community Edition GitLab instance that Fedora uses?
- Votes: +1
- Answer: 
---

- Question: Fedora supports the concept of retired `project` (ie: archived) that no-one can commit to. Does GitLab have an equivalent concept? (The retired status is not something project admins can change)
- Votes: +1 +1 +1 +1
- Answer:
---

- Question: Fedora supports the concept of retired git `branch` (ie: archived) that no-one can commit. Does GitLab have an equivalent concept? (The retired status is not something project admins can change)
- Votes: +1 +1 +1
- Answer:
---

- Question: Extending to above question, the sla's or eol dates are coming from a 3rd party service, when a user pushes to git branch, it will check the eol in that service and rejects it if it is already eol'd. Is this possible in GitLab?
- Votes: +1 +1 +1 +1
- Answer:
---

- Question: Is it possible to hide (or de-emphasize) these retired branches from the UI? Say, hide branch names with f30 or lower?
- Votes: +1 +1
- Answer:
---

- Question: Fedora uses a message bus to integrate different parts of its infrastructure. How should we onboard GitLab into this message bus?
- Votes: +1 +1 +1 +1 +1 +1 +1 +1 +1
- Answer:
---

- Question: Fedora forbids force-push in the main projects but allows them in forks. Would this be feasible in GitLab in a way that people cannot revert?
- Votes: +1 +1 +1 +1 +1
- Answer:
---

- Question: In addition, the above restriction is hardcoded, so even a dist-git administrator can't revert that. This is very useful to avoid cases like https://pagure.io/fedora-infrastructure/issue/9227 . Would be feasible in GitLab to achieve this restriction to gitlab admins?
- Votes: +1 +1 +1 +1
- Answer: 
---

- Question: Fedora supports `+` in repo name, there is a [ticket](https://gitlab.com/gitlab-org/gitlab/-/issues/220309) on it, but it seems to be closed with status being tracked in a private ticket. What is the status on it?
- Votes: +1 +1 +1 +1 +1 +1
- Answer:
---

- Question: What is the bandwidth availability? We need to create branches and commit on the main branch of all the 30k+ repos during certain times of the development cycle. Is that possible and how efficient is it?
- Votes: +1 +1
- Anwer:
---

- Question: Integration with 3rd party tooling? In current dist-git, we can look at the status of the package like whats the latest build in each release, koschei, bz and other links. Orphaned packages can be adopted based on certain rule set. Can we achieve this in GitLab?
- Votes: +1 +1 +1 +1 +1
- Answer:
---

- Question: It is not allowed for users to delete the branches, only certain people with certain access levels are allowed to perform this operation. Is this possible in GitLab?
- Votes: +1 +1 +1
- Answer:
---

- Question: A user is not allowed to rewrite the history. Is it possible? It would be nice to allow only certain people to do so. Although we might not use this, but its good to have just in case if we ever need it.
- Votes: -1 +1
- Answer:
---

- Question: Orphaning a package or repo? A maintainer (owner) of the package can decide to orphan a package, the maintainer should only be able to assign the package to the `orphan` user or any of the other maintainers of the package only. Is this possible to set this branch level as well?
- Votes: +1 +1
- Answer:
---

- Question: Will this lead to better support for Podman in CI workflows in GitLab?
- Votes: 
- Answer:
---

- Question: Will some development effort from Fedora also be put into GitLab?
- Votes: 
- Answer:
---

- Question: Currently dist-git in Fedora has several namespaces: rpms, modules, containers, tests... All namespaces but the ``tests`` namespace have their issue tracker in bugzilla. Would this work in gitlab? Can we selectively enable/disable issue tracking per namespace for the entire instance? (ie: w/o giving the possibility to ``owner`` or ``maintainer`` to toggle that setting.)
- Votes: +1 +1 +1 +1
- Answer:
---

- Question: Can project creation be restricted to a specific group of people in GitLab?
- Votes: +1 +1 +1
- Answer:
---

- Question: Can project (main project, not fork) deletion be restricted to a specific group of people in GitLab? (ie: project owner/maintainer must not be allowed to delete a main project, they can delete their own fork of course)
- Votes: +1 +1 +1 +1
- Answer:
---

- Question: Fedora, as far as I understand, still plan on using bugzilla as issue tracker. Currently default assignee and the CC are gathered using the ``main admin`` (ie: the ``owner`` for GitLab iiuc), the other maintainers (who did not ``unwatch issues`` in the project - mechanism for them to opt-out of being in the CC list) and the people having enabled ``Issue watching`` for the project (mechanism for them to opt-in into being in the CC list). Would this work in a GitLab world?
- Votes: +1 +1 +1
- Answer:
---

- Question: Related to the previous question about bugzilla sync, Fedora's dist-git has grown the possibility to store an "override" of the main admin so a different person/group is set as the default assignee in bugzilla. How could we store that information in GitLab?
- Votes: +1 +1 +1 +1
- Answer:
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
---

- Question: In CentOS, members of a group (say: storage-sig) can push to any branch starting with `c<int>-<sig-name>-*` (so for example, they can push to : c8-storage-sig-v2.0 or c7-storage-sig), but they cannot push to any other branches, like: c8, c7 or any of the branches starting with c8-kernel-sig and so on. Would this be doable in GitLab? (in a way that project owner/maintainer cannot edit?)
- Votes: +1 +1
- Answer:
---

- Question: How would git push over http work with GitLab? (considering gitlab does not have Fedora's password since they are stored in FAS)
- Votes: +1 +1 +1 +1
- Answer:
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
---

- Question: Is this process subject to discussion and aproval by Fedora? The related forum post says that Fedora made a decission, but that's not correct. And there are too much unanswered questions right now for a Fedora decission according to the Changes Policy. So, when/how will we discuss and accept (or not) this on a proper fedorian way?
- Votes: +1
- Answer:
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
---

- Question: In case we move to a GitLab (inc) hosted instance, and after reading https://lists.fedoraproject.org/archives/list/devel-announce@lists.fedoraproject.org/thread/Q74QYG54MNBCY7UX2GITPAZYKHD6BYFH/ , what's the plan around GDPR/CCPA ? Would we lose all historic data due to that? (esp considering Fedora contributors may not agree to share their PI to gitlab)
- Votes: +1 +1
- Answer:

**********************************

[Aoife] Please note: below this line any questions that are added may not be answered during the session. I am sending the above to GitLab now and will publish their answers after the session. Thanks everyone for contributing some really fantastic questions and I'm looking forward to the AMA session tomorrow! :)