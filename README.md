# PoC of the potential cache leakage vulnerability of the liquibase/liquibase repository.

The following are the step-by-step reproduction instructions for the potential cache leakage vulnerability of the [liquibase/liquibase](https://github.com/liquibase/liquibase) repository.

**Note** that since GitHub Actions automatically purges CI task logs older than 90 days [1], the logs for the CI task we used for validation may have been purged. 
This vulnerability has been confirmed and fixed by the liquibase team.


## Attack Notes

Since the victim repository prohibits pull requests from forked repositories from triggering the CI task, it cannot be attacked by a non-collaborator who initiates a pull request. However, there is a non-protected branch (e.g., ```github-action-DAT-14615```) in the repository that can generate a cache containing secrets. Therefore, an unethical collaborator of the repository can launch an attack by submitting code to this non-protected branch, which conforms to our defined threat model. 


## Detailed Steps

Below are the steps for reproducing a cache leakage attack against the liquibase/liquibase repository.

1. The repository owner (i.e., ```cicache-poc```) triggers the execution of a CI task on the ```github-action-DAT-14615``` branch. This task generates a cache that contains secrets, which are ```liquibase-dist/target/keys/datical\_apple.p12``` and ```liquibase-dist/target/keys/datical\_windows.pfx```.
You can find the corresponding log in the "Post Built Code Cache" step of the "Package Artifacts" job.

2. Subsequently, the attacker (i.e., ```lowleveluser-cicache```) triggers a CI task execution on the ```github-action-DAT-14615``` branch and successfully restores the cache file containing secrets. You can find the corresponding log in the "Built Code Cache" step of the "Package Artifacts" job.



## References:
1. Artifact and log retention policy, GitHub Doc. https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#artifact-and-log-retention-policy

