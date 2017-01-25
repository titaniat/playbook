# Contribution Guide

Based on the [thoughtbot code reviews guide](https://thoughtbot.com/playbook/developing/code-reviews).

Contributing to any of our projects must be done according to the following set of steps:

1. Choose an issue to work on. That issue should already exist within the list of issues available within GitLab repo. If you wish to propose a new issue, go ahead!
2. Claim the issue by assigning it to yourself. 
3. Create a new branch and make changes. Remember to commit and push often and meaningfully. Remember to update the issue as you work on it with. Let others know if there have been any problems!
4. When you have finished working on the issue at hand and you have written tests for the code you wrote:
    * Push your code to your remote branch
    * Wait for the build to finish, tests to run and lint to run. Proceed **only** if all of the above pass. That is, make sure that the code builds, tests run correctly and lint passes.
    * Submit a [pull request](https://help.github.com/articles/about-pull-requests/) (on GitHub) or [Merge Request](https://docs.gitlab.com/ee/gitlab-basics/add-merge-request.html) (on GitLab). Remember to include a *meaningful* description of the changes included!
 5. Ask for review on Slack and be responsive to the comments made by others! You should of course review the requests of others. Please use this [review guide](https://github.com/thoughtbot/guides/tree/master/code-review) to give meaningful and concise feedback!
 6. When the review ends and everyone is satisfied:
    * Squash your commits within the branch into a single commit with a good commit message, which summarizes what issue the branch fixes and what are the significant changes within code
      * Rebase onto master
      * Once again, make sure that the build, tests and lints pass (conflict resolution might have influenced this).
      * Let the person with merge permission know that everything is done!
      * You now can delete the local branch containing the changes (make sure not to delete the remote branch!)
7. Let the person with merge privileges:
     * Rebase the branch onto master
     * Delete the remote branch
     
Everything is done and finished!

If you would like to read up on why the review cycle is important, please refer to the bullet list at the end of [this article](https://thoughtbot.com/playbook/developing/code-reviews).
      