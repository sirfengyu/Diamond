
# This workflow will sync DLWorkspace daily update to public repo
# Notice: the branch 'develop' is the main branch for public updates

name: sync_public_daily

on:
  push:
    branches: [ develop ]
  pull_request:
    branches: [ develop ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: merge develop to to_public 
      run: |
        git pull origin develop
        git checkout -b to_public remotes/origin/to_public
        git merge develop
      
    - name: push to_public to github apulis_platform master
      run: |
        # add remote 
        git remote add github https://github.com/apulis/apulis_platform.git
        # directly push 
        git push github master --force
    - name: push to_public to gitee apulis_platform  master
      run: |
        # add remote 
        git remote add gitee https://gitee.com/apulis/apulis_platform.git
        # directly push 
        git push gitee master --force
