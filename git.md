# git
git ===>> version controler and change your application tracking
	github
	gitlab
	sshkey ===>> copy ===>> github
	
	we have 3 stages
		local ===>> local computer
		staging ===>> barzakh
		remote

# install git
	sudo apt update
	apt install git
    sudo apt terminator
    sudo install oh_my_ZSH
    sudo apt curl
	
# git commands
	mkdir workspace

	# more information
	git

	# track directory for git
	git init
		.git ===>> make this directory for git

	git status ===>> show what file make it

	git add file1
	git add . ===>> add file in this file

	git commit -m "add new file" file1
	git commit -m "all linux command" ===>> commit all file
	git commit -am "change file" ===>> add all and commit

	git config global user.email "you@example.com"
	git config global user.name "Your.name"

	# show what happend in the file
	git diff file1

	#make new branch
	git checkout -b dev ===>> make branch and switch to new branch
	git checkout dev ===>> change branch

	git branch stg ===>> make new branch
	git branch ===>> show all branch

	git stash file1

	git log ===>> show all logs
	git reset "commit_id" ===>> role back to last project and delete project
	git checkout "commit_id" ===>> role back to last project

	# change commit if is wrong
	git commit --amend
	git log --oneline ===>> show sumemry
	git log --graph ===>> show in graph
	git branch -d dev ===>> delete brach but first must merge
	git branch -D dev ===>> delete with out question
	git reset file1

	remote
		git remote show origin
		git remote -v
		git remote add origin URL_ADDRESS
		git remote add URL_ADDRESS
		git remote remove origin 
		git push origin main
		git push origin master

		git clone URL_ADDRESS
		git push origin main