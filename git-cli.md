**flags
  -u : track remote branch

---------------------------------------------------------------------------------------------------------

**Stages :
  Stage Area : files are added and begin to be tracked for any changes
  Commit Area : files are added to local repository 

---------------------------------------------------------------------------------------------------------

**Concepts :
  branch : 
  HEAD : the top of the current working branch  
	
---------------------------------------------------------------------------------------------------------

**Commands:
  git add . 	: adds all changes from working directory to staging area and starts tracking.
  git commit 	: saves all changes from staging to local repository.
  git commit -a : directly saves to local repository files from working directory that have been already 
  				  tracked.
  git push 		: saves to remote server all commits stored in local repository
  git fetch 	: gets all commits from remote repository to local repository
  git merge 	: applys all fetched commits 
	
---------------------------------------------------------------------------------------------------------

Merge : the goal is to take commits from a source_branch (e.g. feature) and apply them to a 
		destination_branch (e.g. master)
      : what you will end-up with if you are using merge, is that you will have a messed-up tree, looking 
	  	weierd with all the branching and merging.
			*
			|\
			* *
			| | 
			| *
			|/
			*
			| 
			*
  git checkout master
  git merge feature
	
---------------------------------------------------------------------------------------------------------

Rebase : the goal is to take commits from a source_branch (e.g. feature) and apply them to a 
		 destination_branch (e.g. master) 
       : it is different from merge, it will actually take the commit history of the source_branch and 
	   	 will add it on top of destination_branch. Basically it erases history, because of this it is 
		 hard to trace any changes added into the code-base

			*
   			|
			*
			|
			*
			|
			* 
  git checkout feature
  git rebase master
	
	1st rebase against the feature, this will bring all new commints from master in to the feature and 
	all commits from feature will be moved top of the master's HEAD (resolve conflicts if any)
	
  git checkout master
  git rebase feature

	2nd switch to master and rebase feature, this will bring into master what changes have been added
	to feature.

  git push origin/master
	
	3rd you commit changes remotely 

---------------------------------------------------------------------------------------------------------
 
Cherry-pick : Picks a commit and moves it to current branch. It will use the commitId to identify which
			  commit(s) you want to move.
	      	  Switch to branch to which you want to apply the commit.

  git checkout master
  git cherry-pick commitId
	      
               Flags :
		   --continue : continue cherry-pick after merging conflicts are solved
		   --abord: abord merging conflicts
		   --no-commite : moves the contents of the cherry-picked commit in the current workdir

---------------------------------------------------------------------------------------------------------

Squash : the goal is to merge into one commit multiple commits. You choose the number of commits on which 
		 you want to operate on and you start interactive rebase.
		 Hit 'i' to start edit, and choose which commits are going to be squashed by marking them with 
		 'squash'. 
		 Leave marked with 'pick'the commit into which the other commits are going to be merged into, 
		 usually for avoiding conflicts you will 'pick' the last commit back form the HEAD.

  git rebase -i HEAD~no.
    - where no. is the number of commits you want acces over, starting from HEAD back
	- press 'i' to start editing
	- mark with 'squash' commit(s) to merge 
 	- 'ESC', ':wq', 'Enter' to save

---------------------------------------------------------------------------------------------------------

Amend : let's say you are on a branch and you just commited something, you do a little more work and now
		new changes need to be staged.
        Instead of creating a new commit, and ending up with two commits you can rewrite history using 
		the --amend flag.
	--amend operates on the last commit back from HEAD, and it will add to it the new changes.	
        --no-edit flag indicates that you don't want to rename the last commit message.
	Generates new commit Id

  git commit --amend --no-edit
	
---------------------------------------------------------------------------------------------------------

Reword : let's say you've named your 2nd commit back from HEAD, wrongfully and you want to change its 
		 name, you can use interactive rebase to enter the interactive mode and select the 'reword' option 
		 on the commit which you want to operate. 
	 	 Generates new commit Id for the commit on which you've operated on.
        
  git rebase -i HEAD~no.
    - where no. is the number of commits you want acces over, starting from HEAD back
	- press 'i' to start editing
	- mark with 'reword' commit to rename
 	- 'ESC', ':wq', 'Enter' to save
	- new VIM will open you need to edit again and save in the end.

---------------------------------------------------------------------------------------------------------

Remove : let's say you want to delete a commit. Again you will use interactive rebase and choose the 
		 'drop' to remove the marked commit

  git rebase -i HEAD~no.
    - where no. is the number of commits you want acces over, starting from HEAD back
	- press 'i' to start editing
	- mark with 'drop' commit(s) to delete
 	- 'ESC', ':wq', 'Enter' to save

---------------------------------------------------------------------------------------------------------

Reorder : let's you reorder commits. Again you will use interactive rebase, this time you don't need to 
		  choose any option, you simply switch between commits. 

	- press 'i' to start editing
    - Ctrl-v : copy line
	- d : deletes line
	- 'ESC', ':wq', 'Enter' to save

---------------------------------------------------------------------------------------------------------

Squashing with fixup : just like Squash but will disregard the commit messages of the squashed commits, 
					   keeping only the message of the commit in to which they are squashed.
		       		   Generates new commit Id

  git rebase -i HEAD~no.
    - where no. is the number of commits you want acces over, starting from HEAD back
	- press 'i' to start editing
	- mark with 'fixup' commit(s) you want to squash 
 	- 'ESC', ':wq', 'Enter' to save

---------------------------------------------------------------------------------------------------------

Splitting a commit : it splits a commit into multiple commits

  git rebase -i HEAD~no.
    - where no. is the number of commits you want acces over, starting from HEAD back
	- press 'i' to start editing
	- mark with 'edit' commit you want to split
 	- 'ESC', ':wq', 'Enter' to save

					 You will be in Detached mode
	- git reset HEAD^ (un-stages all files for the given commit)
	- git status to see what was un-staged
	
					 You can now follow a normal flow of adding to stage area and creating commits,
					 still you are in Detached mode
	- git add file_name ...
	- git commit -m "comment" 
	....

					 To go back on the branch you've Detached from
	- git rebase --continue 
---------------------------------------------------------------------------------------------------------

Stash : saves on a stack your changes

  git stash : pushes to stack
  git stash apply : pops from stack the top.
  git stash list : lists all stashed items
  git stash apply 1 : pops from stack the 2nd item from top.


	Display information
  
  git log --online : will display the commitId and commit message

	Rewinde history 

---------------------------------------------------------------------------------------------------------

Checkout commit : checks out a specific commit from a point in time. Safe: read-only action.

  git checkout commitId : checkout to this commit. It deataches from the <branch> on which you are 
  						  currently on
  git checkout <branch> : Reattaches to the HEAD of the branch from which you've left.

---------------------------------------------------------------------------------------------------------

Revert commit : reverts the changes of a specific commit.

  git revert commitId : checkout to this commit. IT deataches from <branch> on which you are currenly on. 
  						Enters VIM in which you see what changes are going to be disregarded and it will 
						create a new commit without those changes.

Reset commit : takes you back in time to a specific commit, discarding all commits after it. 
			   You cannot undo this command.

  git reset HEAD~3 : deletes last 3 commits starting from HEAD back. Retains changes of the commits tha 
  					 are going to be deleted
  git reset commitId : deletes all commits starting from HEAD back to the commit specifide with commitId,
  					   not including it in the reset operation.
		       		   Retains changes of the commits tha are going to be deleted 

	       Flags :
		 --hard : forces a hard reset, meaning that no changes of the commits being deleted, is going 
		 	      to be retained.
