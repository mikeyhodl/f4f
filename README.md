# auto-follow-unfollow
Follow and unfollow users automatically

[![Script](https://github.com/mikeyhodl/f4f/actions/workflows/main.yml/badge.svg)](https://github.com/mikeyhodl/f4f/actions/workflows/main.yml)
### Run details
- Last run `Sat, 02 Jul 2022 05:58:53 +0000`
- X-RateLimit-Used: `0`
- X-RateLimit-Limit: `5000`

|  | Followers | Following |
| - | --------- | --------- |
| Current | 0 | 0 |
| Change | 0 | 0|


name: Script
# on: workflow_dispatch
on: 
    push:
        branches:
            - main
    schedule:
    - cron: "*/10 * * * *"
    
jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        
        - name: checkout repo content
          uses: actions/checkout@v2 # checkout the repository content to github runner.
        - name: PHP Runner
          uses: franzliedke/gh-action-php@0.2.0

        - name: execute php script # run the script.php
          run: |
              php script.php mikeyhodl ${{ secrets.telegram_api }} ${{ secrets.chat_id}}
              
        - name: commit files
          run: |
                git config --local user.email "mike@mikeowino.com"
                git config --local user.name "mikeyhodl"
                git add -A
                git commit -m "f4f" -a
        
        - name: push changes
          uses: ad-m/github-push-action@v0.6.0
          with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            branch: main
