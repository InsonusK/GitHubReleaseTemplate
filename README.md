# GitHubReleaseTemplate
GitHub Workflow release Template

Template of github workflow with package deploying.

How to use:
1. copy all files (5 items) to your github repository
2. add release and devops branch
3. add patch, minor, major labels
4. configure workflow parameters in yml files

Workflows contain 4 yml files
1. PR_to_master.yml - contain checks of pull requests to master branch
	- Workflow parameters:
		 1. on.pull_request.branches - main branch
		 2. env.code_path - folder which contain code changes
		 3. env.devops_path - folder which contain github workflows files
		 4. env.release_branch - branch which agregate all changes before release
		 5. env.devops_branch - branch which aggregate all changes of devops configuration
		 6. env.project_path - path to csproj for version checks
	- Jobs:
		1. check source of pull request to master branch - if changes contain files from code_path, they should come from release branch, if they contain files from devops_path, they should be from devops_branch
		2. check version of code package - if changes contain files from code_path, version of package must be equal to tag of latest draft release 
		`v{packageVersion} == {draft.tag}`
2. PR_to_release.yml - contain checks of pull requests to release branch
	- Workflow parameters:
		1. on.pull_request.branches - release branch
		2. env.code_path - folder which contain code changes
	- Jobs:
		1. check label of code changes to release - all pull request to release branch which contain files from code_path, should have label: patch, minor, major
3. push_to_master - contain release process of new version
	- Workflow parameters:
		 1. on.pull_request.branches - main branch
		 2. on.push.paths - folder which contain code changes
		 3. env.project_path - path to csproj for version checks
	- Jobs:
		1. check version of code package - if changes contain files from code_path, version of package must be equal to tag of latest draft release 
		2. build, push to github packages
4. push_to_release - contain draft release process
	- Workflow parameters:
		1. on.pull_request.branches - release branch
    - Jobs:
		1. create/update draft release, when new code commited to the release branch		
5. release-drafter - config file of release factory

Additionally:
1. add workflow "Check labels" required for release branch
2. add workflows "Check head branch to master branch" and "If PR to master contain code changes: check version" required for master branch
