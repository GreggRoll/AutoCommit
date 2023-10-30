#AutoCommitt
A way to keep that github timeline green. Tested for a month until my secret key expired and testing was completed. 

Runs using a github workflows .yml script.

name: Daily commit

on:
  workflow_dispatch:
  schedule:
    #Here you can change frequency and dates
    - cron: '0 1 * * 1-5'  # Runs at 01:00 from Monday to Friday

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Setup git
      run: |
        #replace with your git config
        git config --global user.name 'your_username'
        git config --global user.email 'your_email'

    - name: Checkout repository
      uses: actions/checkout@v3
      with:
        persist-credentials: false

    - name: Setup PAT
      run: |
        #Set up using repository settings-> secrets and variables-> new repository secret-> named "MY_GITHUB_TOKEN"
        #get your github token with settings-> developer settings-> personal acceess tokens-> generate new token with repo, user, workflow permissions 
        #replace username twice and repository to use your settings
        git remote set-url origin https://username:${{ secrets.MY_GITHUB_TOKEN }}@github.com/username/repository.git

    - name: Create and commit files
      run: |
        RANDOM_COMMIT_COUNT=$(( RANDOM % 10 + 1 ))
        echo "Making $RANDOM_COMMIT_COUNT commits"
        for i in $(seq 1 $RANDOM_COMMIT_COUNT)
        do
        #this will log the commits to a .txt
          echo "$(date) - $i" >> date.txt
          git add date.txt
          git commit -m "Daily commit #$i"
        done
        git push
