# Temporary Quarantine Procedure
In order to allow .NET development to proceed, problems in the build or test that cannot be resolved immediately need a mechanism
to quarantine them in order to unblock the developer workflow of the many developers that contribute to the product.

Quarantine is not the first resort, but it is a tool in order to ensure successful building of the product.

In general if a build or test fails, the steps should be as follows.
1. If the source PR/change can be identified, it should be backed out to restore correct behavior, and then the correction made in a future PR to reinstate the new code
2. If the problem can be definitively fixed quickly, it should be done as soon as possible
3. **If the broken component can be isolated to be removed from PR's and CI builds, it should be quarantined**
4. If none of the above are possible, is should be priority 0 to tackle the situation to get the build unblocked as soon as possible

Step 3 is the focus of this proposal.

## When is something "broken" : \<70% pass rate
We are going to consider something "broken" and in need of remediation **if it has failed 3 of the last 10 builds in the CI pipeline**.
The CI pipeline should be passing 100% of the time, so 3 fails indicates that something needs to be done to unblock PRs.

The quarantine option is meant to be used for issues that are believed to be short term disruptions.  If the fix cannot be determined immediately,
within 5 minutes of the failure, quarantine needs to be enacted to unblock PR workflows.
Permanent unreliability is a different problem not addressed by this procedure.

## What happens when something is quarantined
PR builds will not include the quarantined component.

For now, please follow the existing procedure for that's in place for your repo to disable tests.  The important piece of this document is the intent to keep 'main' passing instead of iterating in 'main' itself.

## How to quarantine
The smallest unit possible should be quarantined, to minimize the coverage gap in PR.

1. A single test in a single configuration
1. A single test in all configurations
1. A test assembly
1. A build "job"
1. An entire pipeline

For now, there's no new guidance regarding the "how" - please continue to follow existing guidelines.  This will be expanded in the future as we work together and learn more.  (issues are opened for this work below)

### A single test in a single configuration
TBD (https://github.com/dotnet/arcade/issues/6661)

### A single test in all configurations
TBD (https://github.com/dotnet/arcade/issues/6661)

### A test assembly
TBD (https://github.com/dotnet/arcade/issues/6662)

### A build job
TBD (https://github.com/dotnet/arcade/issues/6663)

### An entire pipeline
TBD (https://github.com/dotnet/arcade/issues/6663)

## How to reintroduce : Run without failure for 30 days
Once a fix has been introduced, and that component has passed passed for a month, it can be reintroduced into the mainline build by reverting the change
made to quarantine it.

For now this is only being used in the 'staging' pipeline which is used for new platform bring up.  