
### Git Branching Model

#### Branching Model

We are going to use a structured branching model that helps organize development, releases, bug fixes, and hotfixes effectively. Here's a recommended branching model:

1. **Main Branches:**
    - **master:** Represents the main branch where production-ready code resides. This branch should only contain code that has been thoroughly tested and is ready for deployment to production.
    - **develop:** Integration branch for ongoing development. All feature branches should merge into this branch after completion for comprehensive testing before merging into `main`.

2. **Support Branches:**
    - **feature:** Individual feature development branches. These branches are created off `develop` and are used for implementing new features or enhancements. Once complete, they are merged back into `develop`.
        - Example: `feature/new-feature`

    - **hotfix:** Branches for quick fixes to production issues found in `main`. These branches are created off `main`, fixed, tested, and then merged back into both `main` and `develop` to ensure the fix is applied to both.
        - Example: `hotfix/issue-fix`

3. **Release Branches (Optional):**
    - **release:** Branches used for preparing a new production release. These branches are created from `develop` when the code in `develop` is deemed stable for release. Final testing and minor adjustments can be made in this branch before merging into `master` for deployment.
        - Example: `release/v1.0.0`

### Semantic Versioning

Application versioning is a good way to tracking application versions. Semantic versioning (semver) is a versioning scheme that follows a format of MAJOR.MINOR.PATCH:

- **MAJOR:** Increments when you make incompatible API changes.
- **MINOR:** Increments when you add functionality in a backwards-compatible manner.
- **PATCH:** Increments when you make backwards-compatible bug fixes.

For example:
- MAJOR version when you make incompatible API changes,
- MINOR version when you add functionality in a backwards-compatible manner

Certainly! Here's how you can outline the commit message guidelines using the specified format:

### Commit Message Guidelines

#### Format

Commit messages should follow a structured format to provide clear and concise information about each commit. The format generally consists of three parts: type, scope (optional), and subject.

- **Type:** Describes the purpose of the commit. It should be one of the following:
   - `feat`: A new feature or enhancement.
   - `fix`: A bug fix.
   - `docs`: Documentation changes.
   - `style`: Code style changes (whitespace, formatting, etc.).
   - `refactor`: Code refactoring.
   - `build`: Changes that affect the build system or external dependencies like software dependencies version.
   - `ci`: Changes that are related to CI configurations.
   - `test`: Adding or modifying tests.
   - `perf`: A code change that impacts performance testing.
   - `stubs`: Changes to any stubs used in test code.


- **Scope (Optional):** Specifies the scope of the commit, such as a module, component, or feature affected by the change / JIRA confluence ticket number.

- **Subject:** A brief description of the change in the imperative mood (e.g., "fix", not "fixed" or "fixes").

#### Example

```
feat (JIRA-112): add OAuth2 login support
```
- This commit adds OAuth2 login support to the authentication module.

```
fix (JIRA-115): resolve null pointer exception in checkout process
```
- This commit fixes a null pointer exception that occurred in the checkout process.

```
docs: update README with installation instructions
```
- This commit updates the README file to include detailed installation instructions.

#### Guidelines

1. **Separate subject from body with a blank line:** After the subject, leave a blank line before providing a more detailed explanation (if necessary).

2. **Use the imperative mood in the subject line:** Start the subject line with a verb that describes what the commit does.

3. **Limit the subject line to 50 characters:** Keep the subject concise and focused. If more detail is needed, provide it in the body of the commit message.

4. **Use the body to explain what and why vs. how:** Provide context and reasoning for the change in the commit body, if needed.

5. **Reference Jira or issue tracking IDs:** If your project uses issue tracking software, include the issue or ticket ID in the commit message.

#### Benefits

- **Clarity:** Structured commit messages make it easier for team members to understand the purpose of each change.

- **History:** Well-crafted commit messages create a clear history of changes, which is invaluable for future reference and troubleshooting.

- **Automation:** Consistent formatting enables automation of processes like generating release notes or tracking issue resolution.

By following these guidelines, you ensure that your commit messages are informative, consistent, and useful to everyone involved in the project, regardless of the programming language or framework being used. Adjust the specific types and scopes according to your project's needs and conventions.

### Rules of Commits and PRs

#### Commit Guidelines

1. **Atomic Commits:** Each commit should represent a single logical change. Avoid bundling unrelated changes into a single commit.

2. **Clear and Concise Messages:** Use descriptive commit messages that succinctly describe the purpose of the change. Follow the commit message format guidelines previously outlined.

3. **Code Quality:** Ensure each commit adheres to coding standards, is well-documented (if applicable), and passes all relevant tests.

4. **Use Branches:** Create a new branch for each feature or bug fix. Commit changes related to that branch only.

#### Number of Commits Before PR

1. **Small, Frequent Commits:** Make small, incremental commits as you work on a feature or fix. This helps in managing changes and reviewing them efficiently.

2. **Logical Grouping:** Group commits logically based on the scope of changes. Aim to have commits that make sense as standalone units of work.

3. **Avoid Excessive Commits:** While there is no strict limit on the number of commits, avoid making too many small, insignificant commits. Instead, strive for meaningful commits that reflect substantial progress or a complete unit of work.

4. **Quality Over Quantity:** Focus on the quality and clarity of each commit message rather than the sheer number of commits.

### Benefits of PRs

- **Code Review:** PRs facilitate thorough code review, allowing team members to provide feedback and catch potential issues early.

- **Integration Testing:** PRs provide an opportunity to integrate changes into the main codebase in a controlled manner, ensuring compatibility and stability.

- **Documentation:** PRs often include documentation updates, ensuring that changes are well-documented and accessible to all team members.


By following these guidelines, developers can effectively manage their commits, maintain code quality, and streamline the process of integrating changes into the main codebase through Pull Requests. Adjust these guidelines based on your team's specific workflow and project requirements.