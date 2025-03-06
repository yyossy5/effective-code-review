- [Code Review Tips for Reviewees](#code-review-tips-for-reviewees)
  - [Reply When Making Changes Based on Comments and Include Commit Links](#reply-when-making-changes-based-on-comments-and-include-commit-links)
  - [Reviewer Comments are Important but Not All Need to be Accepted](#reviewer-comments-are-important-but-not-all-need-to-be-accepted)
- [Code Review Tips for Reviewers](#code-review-tips-for-reviewers)
  - [Provide Prompt Feedback](#provide-prompt-feedback)
    - [Impact of Code Review Delays](#impact-of-code-review-delays)
    - [Expected Practices](#expected-practices)
  - [Avoid Fragmented Comments](#avoid-fragmented-comments)
  - [The Most Fundamental Question Code Review Should Answer: Can I Maintain This?\[2\]](#the-most-fundamental-question-code-review-should-answer-can-i-maintain-this2)
  - [Respect Code Authors and Ask Questions](#respect-code-authors-and-ask-questions)
  - [Be Polite\[1\]\[2\]](#be-polite12)
  - [Don't Point Out Issues Without Alternatives, and Provide Specific Suggestions](#dont-point-out-issues-without-alternatives-and-provide-specific-suggestions)
  - [Label Comments\[8\]](#label-comments8)
    - [Label Examples](#label-examples)
    - [Benefits of Using Labels](#benefits-of-using-labels)
  - [Review Code in Your Own Editor\[2\]\[5\]](#review-code-in-your-own-editor25)
  - [Praise Good Parts and Express Gratitude](#praise-good-parts-and-express-gratitude)
- [Code Review Tips for Both Reviewees \& Reviewers](#code-review-tips-for-both-reviewees--reviewers)
  - [Keep PR-Related Discussions on the PR](#keep-pr-related-discussions-on-the-pr)
  - [Implement Automated Static Analysis to Reduce Review Burden\[1\]](#implement-automated-static-analysis-to-reduce-review-burden1)


# Code Review Tips for Reviewees

## Reply When Making Changes Based on Comments and Include Commit Links

- When addressing comments, always reply so reviewers know about the changes without having to proactively check the PR
- Include links to the commits that address the comments in your replies, making it immediately clear which commit addresses which comment, greatly facilitating the review process
    - Instead of combining multiple comment responses into one commit, separate commits by topic
- Without this, having many commits with messages like "Address review comments" makes it difficult to find which commit addresses which comment

## Reviewer Comments are Important but Not All Need to be Accepted

- If you (the reviewee) disagree with or have questions about comment content, ask questions and engage in dialogue
- However, code authors are in the state of having the most context having just completed the code, so they're not in a position to evaluate aspects like code readability. Reviewers are reading with less context, so their opinions shouldn't be taken lightly

# Code Review Tips for Reviewers

## Provide Prompt Feedback

### Impact of Code Review Delays

| Impact Area | Specific Impact | Details |
|------------|----------------|---------|
| Development Process | Reduced Development Speed | - Delayed merging of changes extends development cycles<br>- Delays in new feature releases and bug fixes |
| | Increased Context Switching Cost | - Time needed to regain context when returning to long-waiting PRs<br>- Reduced productivity and potential rework |
| | Increased Branch Conflict Risk | - Higher risk of long-unmerged branches conflicting with main branch<br>- Additional time and resources needed for conflict resolution |
| Team Dynamics | Decreased Motivation | - Motivation drop due to lack of feedback<br>- Negative impact on team communication and collaboration |
| | Extended Feedback Loops | - Lost opportunities for early feedback<br>- Repetition of same issues and slower learning curve |
| | Hindered Team Coordination | - Changes dependent on other teams stuck in review<br>- Impact on overall project progress |
| Code Quality | Negative Impact on Code Quality | - Missing details when reviewing large amounts of code at once<br>- Missed bugs and overall code quality decline |
| | Technical Debt Accumulation | - Need for changes due to requirement/design changes during review wait<br>- Debt accumulation from changes needing immediate post-merge fixes |
| | Increased Security Risks | - Delays in security-related fixes and updates<br>- Risk of prolonged vulnerability exposure |
| Project Management | Project Management Complexity | - Difficulty tracking project progress with accumulated unreviewed PRs<br>- Complicated resource allocation and deadline management |
| Business Impact | Customer Satisfaction Impact | - Decreased customer satisfaction from delayed feature/fix releases<br>- Increased risk of falling behind competitors |

### Expected Practices

- Best to provide first review within 24 hours[1]
  - If it will take time, at least reply that you've seen the PR and will start review soon (reviewees get anxious if there's no response for a long time)
    - The sooner the better, but generally no need to interrupt focused work to start immediately (might reduce overall efficiency)[6]
- Good to incorporate code review into work routine[2]
    - For example, reserve 30 minutes each morning and after lunch for reviews
    - Could take an hour before lunch, or time before going home. However, more likely to skip if in the middle of something else

## Avoid Fragmented Comments

- *Reviewers should avoid responding to the code review in piecemeal fashion. Few things annoy an author more than getting feedback from a review, addressing it, and then continuing to get unrelated further feedback in the review process.*[1]
- While it's easier for the reviewer to send feedback as they think of it, this can be mentally exhausting for the recipient in some cases, so care is needed
- Use the Start Review feature in review tools
    - Allows sending multiple comments together at once

## The Most Fundamental Question Code Review Should Answer: Can I Maintain This?[2]

- It's not an overstatement to say code readability is the most important criterion for code review. Does the code fit in your brain?
- Are methods too long or complex? Is it easy to understand what they're doing?
- *Surprisingly enough, checking for code correctness is not the primary benefit Google accrues from the process of code review. (...) but more importance is attached to ensuring that a code change is understandable and makes sense over time and as the codebase itself scales.*[1]

## Respect Code Authors and Ask Questions

- *In general, reviewers should defer to authors on particular approaches and only point out alternatives if the author’s approach is deficient. If an author can demonstrate that several approaches are equally valid, the reviewer should accept the preference of the author. (...) It’s better to ask questions on why something was done the way it was before assuming that approach is wrong.*[1]
- Be careful not to impose mere preferences

## Be Polite[1][2]

- Code review is an asynchronous process, typically done through text. Tone and intent are easily lost in text. Even unintentionally harsh wording can hurt the recipient. Be extra polite.

## Don't Point Out Issues Without Alternatives, and Provide Specific Suggestions

- Just pointing out issues might leave reviewees at a loss, so at minimum communicate your idea for fixes
- If suggestions are vague, reviewees might struggle again, so be specific when possible
- Also, being specific helps prevent situations where you hesitate to re-comment because the reviewee's changes based on your comment differ slightly from what you had in mind

## Label Comments[8]

### Label Examples

| Label | Meaning | Intent |
|-------|---------|--------|
| ASK | Question | Question requiring an answer |
| FYI | For your information | Sharing for reference. No action needed |
| NITS | Nitpick | Minor points. Can be ignored |
| NR | No rush | Suggestions that don't need immediate action but should be considered for future. Consider creating tasks or fixes |
| IMHO | In my humble opinion | Indicates personal opinion open for discussion |
| MUST | Must | Mandatory fixes required for approval |

### Benefits of Using Labels

- Not all comments require fixes, leading to more equal relationships
- Clear communication of required actions prevents misunderstandings
- Clear prioritization of comment responses
- Labels like nits and imho reduce risk of feedback being taken as overly critical
- Lower barrier to commenting promotes active discussion
- Can analyze code quality trends by analyzing labeled comments
    - For example, high frequency of ask labels might indicate insufficient explanation in comments or commit messages

## Review Code in Your Own Editor[2][5]

- Generally good to check out code on your own machine for review
- Can notice things not apparent when just viewing in GitHub or Bitbucket browser
  - Formatter not applied
  - Analyzer warnings
  - Missed changes
    - Naturally seeing surrounding code can help notice missed changes (e.g., magic numbers still remaining during constant extraction)
- Checking out code on your machine helps understand changes in whole project context, verify your suggested fixes, and take more responsibility than just commenting
- Also helpful when using services like Bitbucket that don't support function jumping in browser
- For VSCode, reviewing with GitLens extension is recommended

## Praise Good Parts and Express Gratitude

- *When you see something nice in the CL, tell the developer, especially when they addressed one of your comments in a great way. Code reviews often focus on mistakes, but they should also highlight good practices. It's sometimes even more valuable to a developer's learning to tell them what they did right than to tell them what they did wrong.*[6]

# Code Review Tips for Both Reviewees & Reviewers

## Keep PR-Related Discussions on the PR

- Discussions should be kept on PRs as scattered discussion locations are undesirable
- If discussed on Slack or verbally, link or note it in PR comments so others can understand later

## Implement Automated Static Analysis to Reduce Review Burden[1]

- Removing minor issues through static analysis beforehand helps reviewers focus more on essential reviews
