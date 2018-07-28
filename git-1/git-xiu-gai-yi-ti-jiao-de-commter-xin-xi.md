# git修改已提交的Commter信息

git filter-branch -f --env-filter "GIT\_AUTHOR\_NAME='Newname'; GIT\_AUTHOR\_EMAIL='newemail'; GIT\_COMMITTER\_NAME='Newname'; GIT\_COMMITTER\_EMAIL='newemail';" HEAD

git filter-branch -f --env-filter "GIT\_AUTHOR\_NAME='lozvx'; GIT\_AUTHOR\_EMAIL='lozvxx@gmail.com'; GIT\_COMMITTER\_NAME='lozvx'; GIT\_COMMITTER\_EMAIL='lozvxx@gmail.com';" HEAD

