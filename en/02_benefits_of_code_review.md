- [Benefits of Code Review\[1\]](#benefits-of-code-review1)
  - [Verifying Code Correctness](#verifying-code-correctness)
  - [Building Shared Understanding of Code](#building-shared-understanding-of-code)
  - [Maintaining Codebase Consistency](#maintaining-codebase-consistency)
  - [Psychological Benefits](#psychological-benefits)
  - [Knowledge Sharing and Discussion History](#knowledge-sharing-and-discussion-history)
- [Benefits of Efficient Code Review Process](#benefits-of-efficient-code-review-process)
- [Key Principles for Better Review Process](#key-principles-for-better-review-process)
  - [PR Submitter (Reviewee)](#pr-submitter-reviewee)
  - [PR Reviewer](#pr-reviewer)


# Benefits of Code Review[1]

## Verifying Code Correctness

- Detecting defects early in the process naturally reduces the time needed for later fixes[1][12]
    - Code review saves time that would otherwise be spent on debugging and manual testing
    - However, this is only true when the code review process itself remains lightweight

## Building Shared Understanding of Code

- Since code is read far more times than it is written, ensuring everyone can understand it is crucial[1]

- What only non-authors can do: Provide feedback unaffected by the author's perspective bias
    - Questions about code meaning become increasingly costly to address over time, so authors should consider each review question as valid and respond accordingly
    - This doesn't necessarily mean changing approach or logic based on criticism, but rather potentially needing to explain your code more clearly

## Maintaining Codebase Consistency

- Consistent writing style across the team makes code more understandable and readable (even for engineers from other teams unfamiliar with the codebase)
- Following language best practices and ensuring code doesn't become overly complex
- Consistent and simple code is relatively easy to understand and refactor
- While consistency might sometimes conflict with functionality, changes that maintain consistency with existing code while keeping complexity low can be preferable

## Psychological Benefits

- Prevents engineers from developing personal styles or individual approaches to software design
    - Code is part of the business, not personal property
- The code review process itself helps soften criticism
    - While code reviews inherently require critical opinions, well-functioning reviews present objections in a prescribed, neutral way that softens criticism
- Code authors become more mindful of their changes
    - Without code reviews, they might cut corners
        - Example: "I haven't done all unit tests, but I can do them later..." Code reviews make authors more conscious of such issues through peer checking
    - The existence of code reviews provides authors a good opportunity to review their own changes and ensure everything is properly prepared before submission
        - This leads to quality improvements

## Knowledge Sharing and Discussion History

- Code reviews are perfect opportunities for knowledge transfer
- The code review process is a primary way for engineers to interact and exchange coding techniques
- Code reviews serve as historical records
    - For example, any engineer can later investigate details about how specific designs were introduced

# Benefits of Efficient Code Review Process

- Improved team-wide productivity
    - Efficient review processes with quick reviewer feedback accelerate development
    - Less review wait time reduces context switching impact, improving individual developer productivity
    - (Conversely, having many PRs that are difficult to review can decrease team-wide productivity)
- Quality improvement
    - Reviewers can focus more on essential issues (like business logic), making it harder to miss important problems
    - Early problem detection and correction helps control technical debt
        - When reviews take too long, maintainability might be compromised to meet feature release schedules
- Positive team impact
    - Reduced review-related frustration
    - Sense of achievement through reviewer-reviewee collaboration
    - Effective sharing of best practices and design patterns, enabling mutual growth

These benefits lead to business value improvement through rapid feature releases and increased customer satisfaction from high-quality code.


# Key Principles for Better Review Process

## PR Submitter (Reviewee)

Enable reviewers to focus on essential issues (like business logic).

To achieve this, write understandable code. Create small PRs focused on a single purpose. Use appropriate commit units, code comments, commit messages, and PR descriptions.

## PR Reviewer

Provide prompt feedback. Respect code authors and ask questions. When making suggestions, show reasoning and provide specific alternatives to avoid causing difficulties for others.
