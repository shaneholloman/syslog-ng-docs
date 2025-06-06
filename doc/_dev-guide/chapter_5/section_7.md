---
title: Process
id: dev-process
---

[ar:issue-tracker]: https://github.com/balabit/syslog-ng/issues 

Of course, we also accept patches. If you want to submit a patch, the
guidelines the following:

 1. Open an issue first, if there is none open for the particular
    issue for the topic already in the
    [issue tracker][ar:issue-tracker]. That way, other contributors
    and developers can comment on the issue and the patch.
 2. If you submit a pull request that fixes an existing issue, mention
    the issue somewhere in the pull request, so we can close the
    original issue as well.
 3. We are using a coding style very similar to
    [GNU Coding Standards](https://www.gnu.org/prep/standards/standards.html#Writing-C)
    for syslog-ng. Please try to follow the existing conventions.
 4. Always add a `Signed-off-by` tag to the end of **every** commit
    message you submit.
    Signing off on commits enables users to affirm that a commit complies with the rules and licensing of a repository. This, in the case of {{ site.product.short_name }}, means that you accept our coding and contribution standards described in the Contribution section, and accept that your code will be published with a `GPLv2` license.
 5. Always create a separate branch for the pull request, forked off
    from the {{ site.product.short_name }} `develop` branch.
   The pull requests you create should always point to and be merged into the `develop` branch. Do not make PRs and try to merge into the `master` branch.
 6. If your patch should be applied to multiple branches, submit
    against the latest one only, and mention which other branches are
    affected. There is no need to submit pull requests for each
    branch.
 7. If possible, write tests. We love tests.
 8. A well-documented pull request is much easier to review and merge.
