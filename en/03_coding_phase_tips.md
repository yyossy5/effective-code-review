# Coding Phase Tips

- [Coding Phase Tips](#coding-phase-tips)
  - [Properly Abstract Code to Keep it Small](#properly-abstract-code-to-keep-it-small)
    - [Cyclomatic Complexity\[2\]](#cyclomatic-complexity2)
    - [80/24 Rule\[2\]](#8024-rule2)
    - [How to Abstract Properly](#how-to-abstract-properly)
    - [Code Example Before Abstraction (Cyclomatic Complexity 11)](#code-example-before-abstraction-cyclomatic-complexity-11)
    - [Code Example After Abstraction (Cyclomatic Complexity 1)](#code-example-after-abstraction-cyclomatic-complexity-1)
  - [Write Good Change Descriptions](#write-good-change-descriptions)
    - [Write Appropriate Commit Messages](#write-appropriate-commit-messages)
      - [Benefits of Writing Appropriate Commit Messages](#benefits-of-writing-appropriate-commit-messages)
      - [Git Commit Message Standard: 50/72 Rule\[2\]\[3\]](#git-commit-message-standard-5072-rule23)
      - [Examples of Poor Commit Messages](#examples-of-poor-commit-messages)
      - [Examples of Good Commit Messages](#examples-of-good-commit-messages)
    - [Write Code Comments](#write-code-comments)
    - [Code Comments vs Commit Messages vs PR Description?](#code-comments-vs-commit-messages-vs-pr-description)
      - [Places to Write Information Not Apparent in Code\[14\]](#places-to-write-information-not-apparent-in-code14)
      - [Distinguishing Between Code Comments and Commit Messages](#distinguishing-between-code-comments-and-commit-messages)
      - [What to Write in PR Description and Comments](#what-to-write-in-pr-description-and-comments)
  - [Pay Attention to Commit Units](#pay-attention-to-commit-units)
  - [Add Prefixes to Commit Messages\[7\]](#add-prefixes-to-commit-messages7)
  - [Consider Rebase Instead of Merge When Incorporating Changes from Target Branch\[4\]\[13\]](#consider-rebase-instead-of-merge-when-incorporating-changes-from-target-branch413)
  - [Make Good Use of Draft PRs](#make-good-use-of-draft-prs)
  - [Command Query Separation (CQS)\[2\]](#command-query-separation-cqs2)
  - [Remember That Code is Debt\[1\]](#remember-that-code-is-debt1)


## Properly Abstract Code to Keep it Small

Always be mindful of code complexity.
- Functions that try to do too much become large and reduce readability and testability
- It's beneficial to properly abstract and delegate to separate classes or functions based on concerns

### Cyclomatic Complexity[2]
- A metric for code complexity that counts code paths
- Start with 1 and increment for each branching and loop instruction like if and for
- Count keywords and increment the number (starting from 1) for each occurrence
- If it exceeds 7, the function is too complex and should be refactored
    - Human short-term memory can only hold 4-7 pieces of information[10][11]

<br>
Examples shown below

### 80/24 Rule[2]
- It's good to have guidelines for maximum line width/method length
- Line width: 80 characters
- Maximum method length: 24 lines
- The numbers 80/24 themselves aren't crucial. The point is to avoid excessive line width so you can view unit tests and test targets side by side, or view diffs in side-by-side view. Also, keeping methods short vertically allows you to see the entire method without scrolling

While 24 lines might be challenging in practice, the key is being conscious of whether functions are becoming too long and difficult to understand (hard to grasp what they're doing).

### How to Abstract Properly
- Extract the essence: Extract cohesive processing into a single method
    - If the intention of a method is clear within the flow of processing without seeing implementation details, the abstraction is successful
- Don't try to do everything in one function; create classes for each responsibility and delegate processing

### Code Example Before Abstraction (Cyclomatic Complexity 11)

```python
def calculate_discounted_price(total_price, user_type, is_holiday, coupon_code):
    discount = 0

    if user_type == "premium":
        if total_price >= 100:
            discount = 0.20
        elif total_price >= 50:
            discount = 0.15
        else:
            discount = 0.10
    elif user_type == "regular":
        if total_price >= 100:
            discount = 0.10
        elif total_price >= 50:
            discount = 0.05
        else:
            discount = 0

    if is_holiday:
        discount += 0.05

    if coupon_code == "SAVE10":
        if discount < 0.10:
            discount = 0.10
    elif coupon_code == "SAVE20" and total_price >= 200:
        discount = max(discount, 0.20)

    return total_price * (1 - discount)

# Usage example
price = calculate_discounted_price(120, "premium", True, "SAVE20")
print(f"Discounted price: {price}")
```

### Code Example After Abstraction (Cyclomatic Complexity 1)

```python
def _get_base_discount(total_price, user_type):
    discounts = {
        "premium": [(100, 0.20), (50, 0.15), (0, 0.10)],
        "regular": [(100, 0.10), (50, 0.05), (0, 0)]
    }
    for threshold, discount in discounts[user_type]:
        if total_price >= threshold:
            return discount

def _apply_holiday_discount(discount, is_holiday):
    return discount + 0.05 if is_holiday else discount

def _apply_coupon(discount, total_price, coupon_code):
    if coupon_code == "SAVE10":
        return max(discount, 0.10)
    elif coupon_code == "SAVE20" and total_price >= 200:
        return max(discount, 0.20)
    return discount

def calculate_discounted_price(total_price, user_type, is_holiday, coupon_code):
    discount = _get_base_discount(total_price, user_type)
    discount = _apply_holiday_discount(discount, is_holiday)
    discount = _apply_coupon(discount, total_price, coupon_code)
    return total_price * (1 - discount)

# Usage example
price = calculate_discounted_price(120, "premium", True, "SAVE20")
print(f"Discounted price: {price}")
```

## Write Good Change Descriptions

### Write Appropriate Commit Messages

- Commit messages are communication and should be written properly for team members and your future self
- Convey your thoughts that aren't apparent in the code diff, such as the intention behind changes

#### Benefits of Writing Appropriate Commit Messages

| Benefit | Details |
|---------|---------|
| More Efficient Code Reviews | ・Reviewers can more easily review by seeing diffs and author's decision-making (reasons for changes) together per commit[9]<br>・The act of trying to write appropriate commit messages encourages small, focused commits<br>・Reduces communication costs between reviewee and reviewer |
| Improved Maintainability | ・Makes reasons for code changes clear, serving as reference for future modifications<br>・Answers questions like "why was it implemented this way?" |
| Documentation Complement | ・Serves as a place to record detailed implementation and decisions<br>・Can reduce code comments and improve code readability |
| Code Quality Improvement | ・Writing commit messages provides an opportunity to reconsider and improve your changes |
| Easier PR Description Writing | ・With appropriate commit units and messages, changes are easier to understand even for yourself (no need to recall change reasons as they're recorded in commit messages) |

#### Git Commit Message Standard: 50/72 Rule[2][3]

- What is the 50/72 rule
    - Write a summary in present tense within 50 characters (about 25 characters for Japanese)
    - If adding more text, leave the second line blank
    - Add as much text as needed, but format to stay within 72 characters width
- Benefits of the 50/72 rule
    - Easy to read in git log --oneline and tools like GitHub, Bitbucket
    - Writing in present tense tends to make lines shorter (more readable)
- Commit message content
    - Summary alone is fine when code is self-explanatory
    - Add context if there might be any questions
    - Since commit diffs already show what and how things changed, commit messages are ideal for explaining why changes were made and why they took their current form

#### Examples of Poor Commit Messages

```text
Login-related fixes
Implemented login attempt limit. For hacker prevention. Also fixed password reset bug. Updated related tests.
```

- Vague summary. Should clearly indicate main changes as specific content isn't clear from commit message alone
- No blank second line making it hard to distinguish summary from details
- Too wide horizontally, reducing readability
- Multiple independent changes in one commit
- Insufficient explanation
  - "For hacker prevention" is too simple. Should explain more specifically what security improvements are expected

<br><br>
Other common examples needing improvement:

- "Address review comments"
    - With multiple comments, reviewers might need to figure out which comment is being addressed. Also unclear what was fixed when looking back later
- "Fix bug"
    - *This alone isn't helpful for reviewers or future code archaeologists*[1]
    - Can't tell what bug was fixed without looking at commit contents

#### Examples of Good Commit Messages

```text
Implement login attempt limit per user

Added functionality to limit login attempts per user
within a specific time period. This enhances system
security by preventing brute force attacks.
```

- Appropriate commit granularity
  - Does only one thing, so purpose and diff completely correspond
  - Not mixing multiple different changes in one commit makes it easy to revert changes or apply to other branches
- Explains reason for change
  - Helpful for future maintenance (can be used to judge whether similar changes should be made)
- Follows 50/72 rule for high readability in various Git tools

### Write Code Comments

- If reviewers can't understand your change diff or its reasons, even if your changes are correct, the code structure or comments (or both) need improvement[1]
- More noticeable than commit messages, so record important supplementary information or notes that help understand behavior
- At minimum, complete docstrings or JavaDoc for public functions and classes

### Code Comments vs Commit Messages vs PR Description?

#### Places to Write Information Not Apparent in Code[14]

Code comments vs commit messages vs PR (description, comments)

#### Distinguishing Between Code Comments and Commit Messages

Both remain in the codebase, but code comments are more noticeable while commit messages must be actively looked up (generally).

Therefore, information that should be immediately noticeable should be written in code comments.

What to write in code comments:
- Important points to note when modifying that part
    - Explanations for parts that are intentionally counter-intuitive
- Explanations that reduce cognitive load for complex parts
    - Natural language explanations for areas that aren't immediately clear what they're doing

What to write in commit messages (mainly helpful during review):
- Change intention for that diff
- Thoughts worth recording but would be redundant as code comments
    - For example, "There were these other options for achieving this diff's purpose, but chose this method because..."
        - (Whether it should be in more noticeable code comments depends on the situation. Trade-off between readability and content importance)

#### What to Write in PR Description and Comments

Write information likely needed during review

- PR purpose, change overview
- Areas you're unsure about or want focused review on (if any)
- Links to documentation or tickets explaining feature requirements
- Notes about minor inclusions different from PR purpose (like fixing some existing comments while you were there)

<br>

For supplementary explanations about the code itself that should be visible when viewing code in the future, better to write as code comments (or include in commit messages) that remain in the codebase rather than explaining in PR comments

## Pay Attention to Commit Units

Make commit units meaningful groupings. Specifically, it's good to have one commit per one decision[9]

- For example, don't mix different feature additions in one commit (makes it hard to tell which parts of the diff are for which purpose). Also don't mix changes that don't match the commit message
  - ❌ "Enable A (actually enables B too)"
  - ⭕ "Enable A" "Enable B"
- "One decision" granularity: Think of it as one component needed to achieve the PR's purpose
- If commit units are appropriate, commit messages become easier to write and prefixes (discussed later) can be applied well

<br>
Benefits of making commit units per decision ↓

- With appropriate commit units and messages, you can see diffs and their intentions together (enabling per-commit review) making review much easier
- When commits are per decision, actions like "want to undo this so reset" become easy

<br>

It's good to be able to develop while consolidating commits as needed using interactive rebase and amend commit.
- For example, if there are three commits like add feature A, fix it, fix it again, these would be better consolidated into one for a cleaner tree and much easier review

Reference: [Appendix: Commit related techniques](./Appendix_commit_related_techniques.md)

## Add Prefixes to Commit Messages[7]

- Adding prefixes to commit messages makes the type of change instantly clear, making review easier
    - Example: "feat: Enable ◯◯"
    - For instance, easy to quickly identify which commits you want to look at when browsing commit list
- Makes developers more conscious of commit granularity
- (The essence isn't correctly applying prefixes, but the effect of making you conscious about commit units and readability (so don't worry too much about classification))

Prefixes used in Angular project (https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#type)
```
feat: A new feature
fix: A bug fix
docs: Documentation only changes
style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
refactor: A code change that neither fixes a bug nor adds a feature
perf: A code change that improves performance
test: Adding missing or correcting existing tests
chore: Changes to the build process or auxiliary tools and libraries such as documentation generation
```

## Consider Rebase Instead of Merge When Incorporating Changes from Target Branch[4][13]

- Rebase avoids creating unnecessary merge commits
- Merge commits to feature branches become noise making the actual modification history harder to understand
    - Not so hard to see in GUIs like Bitbucket, but quite messy in git log as it shows logs for each commit within merge commits
    - Rebase makes commit tree more linear and understandable

<br>

Notes
- Since rebase rewrites history, use merge or get agreement when working on branches shared with other developers
- Merge is preferable after PR submission
    - After PR submission, other developers may be reviewing or testing, so safer not to change history
    - Rebase changes commit hashes making commit links outdated
    - Basically better to avoid rebase when others might have pulled

References:
- [Merging vs rebasing - Atlassian](https://www.atlassian.com/ja/git/tutorials/merging-vs-rebasing)
- [Appendix: Commit related techniques](./Appendix_commit_related_techniques.md)

## Make Good Use of Draft PRs

(Repeated) Detecting defects early in the process naturally reduces the time needed for later fixes[1][12]
<br>

- To prevent major rework, use draft PRs to consult with team members when you want to discuss something even during development
- While Bitbucket etc. don't have draft PR features, similar usage is possible by not setting reviewers
- Or good to simply discuss while viewing diffs from branch screen without specifically creating draft PR

## Command Query Separation (CQS)[2]

- Command: Methods with side effects
	- Side effect: When a procedure changes some state
	- Methods with side effects should not return data, meaning return type should be void
        - If a method doesn't return data, you know it's a method with side effects
- Query: Methods without side effects that return data
	- With CQS, if a method has a return type, you can understand it has no side effects
- Separating commands and queries makes API intentions easier to understand. And you can distinguish between two types of functionality without reading implementation code
- Queries are easier to understand than commands, so prioritize queries over commands

## Remember That Code is Debt[1]

- *It might be a necessary liability, but by itself, code is simply a maintenance task to someone somewhere down the line. Much like the fuel that an airplane carries, it has weight, though it is, of course, necessary for that airplane to fly.[1]*
    - Be mindful of whether there's sufficient justification for new features, and watch out for code duplication, reinventing the wheel, etc.
- The more lines of code, the more the codebase deteriorates[2]
	- The more code you add, the more people need to read and understand
- When trying to create new processing from scratch, first consider whether it can be done with existing code or libraries
  - Of course, don't force using existing code when it would make things harder to understand

</rewritten_file>
