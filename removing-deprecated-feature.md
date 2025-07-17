# Removing Deprecated Feature

I wrote this guide on some learings when deprecating a feature that were not fully familiar.

## Opt to have an POC

This will give an idea how we can break down the parts to remove.
In turn, allow us to communicate with other teams if it requires some other steps we are not aware of. By the time we actually do it; we have everything setup.

## Opt to have multiple small PR with similar concerns

This reduces code review fatigue and focuses the area to test on.
This also makes it easier to spot that it's our PR that caused the issue.

An example could look like the following;
where each of the list would be its own PR.

* Page components
* Shared components
* Controllers
* Concerns
* Helpers
* Applications
* Jobs
* Models
* Backend tests
* Frontend tests

Let's say that the Page components happen to be 100 files. Then it does not hurt to subdivide it further especially if we still want it to semi work. The most important thing is that it's easier to review.

The main enemy of this kind of work is having someone willing to review the PR.

## Opt to have the smallest effort PR reviewed first

By nature removing code tends to be done in order. It's best to remove the easiest first to get results right away.

For example having the PR's that require approval from other teams could be done in the end.

In the example I gave beforehand. The Page components could be done first since it only requires our team's approval and we are familiar with a lot about these components. Application changes could be done further away since it requires other teams approval.

## Opt to have a maximum of 1 approval from other team per PR

This reduce the time for approval for a PR, lessens the pressure for a reviewer to get it done in a hurry. In turn gives more time for the reviewer to spot critical issues in the code.
