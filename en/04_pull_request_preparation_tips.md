# Pull Request Preparation and Creation

- [Pull Request Preparation and Creation](#pull-request-preparation-and-creation)
  - [Create Small PRs That Do One Thing](#create-small-prs-that-do-one-thing)
    - [Issues Caused by Multiple Purposes in One PR](#issues-caused-by-multiple-purposes-in-one-pr)
  - [Conduct Self-Review Before PR Submission](#conduct-self-review-before-pr-submission)


## Create Small PRs That Do One Thing

Create PRs for each single purpose (feature addition, bug fix, refactoring, etc.).

(And the commit history becomes a stack of multiple decisions made to achieve that single purpose)

### Issues Caused by Multiple Purposes in One PR

- Being large makes review very difficult, so reviewers are more likely to reject or postpone it
    - Longer wait times increase context switching costs
    - Conflict resolution costs increase
- Cannot merge parts that are fine[4]
    - Consider a PR containing three purposes A, B, and C, where the reviewer agrees with A and B but raises discussion about C. Then, even though the code changes for A and B are fine, they can't be merged while C's discussion continues

However, in reality, there might be situations where you want to submit multiple purposes in one PR (main purpose + small side purposes, etc.). In such cases, making review easier by explaining in the PR description or adding your own comments to the diff. If the commit history is clean (easy to review commit by commit), the PR might still be okay even if it's somewhat large

## Conduct Self-Review Before PR Submission

- Surprisingly, you can often find things that should be improved yourself when conducting self-review before PR submission
- It's particularly good to find non-essential issues yourself, like typos or remaining TODO comments
    - When reviewers can focus more on essential issues (like business logic), it reduces review effort, makes problems less likely to be missed, and leads to a better review process
    - (By the way, if it's before PR submission, rather than leaving commits that just fix typos, integrating those fixes into the commits of the modified sections creates a cleaner and easier to review commit history without unnecessary commits)
