# GitHub Actions pro pravidelné ukládání dat o očkování

name: Automaticke stahovani dat z MZ CR

on:
  push:
  workflow_dispatch:
  schedule:
    - cron: '*/5 * * * *'

jobs:
  scheduled:
    runs-on: ubuntu-latest
    steps:
    - name: Propojeni s repozitarem
      uses: actions/checkout@v2
    - name: Přehled vykázaných očkování podle krajů ČR
      run: |-
        wget -q -O ockovani-podle-kraju.csv https://onemocneni-aktualne.mzcr.cz/api/v2/covid-19/ockovani.csv
    - name: Uloz zpravu do repozitare
      run: |-
        git config user.name "Automated"
        git config user.email "actions@users.noreply.github.com"
        git add -A
        timestamp=$(date -u)
        git commit -m "Posledni data: ${timestamp}" || exit 0
        git push
    - name: Přehled vykázaných očkování podle míst
      run: |-
        wget -q -O ockovani-podle-mist.csv https://onemocneni-aktualne.mzcr.cz/api/v2/covid-19/ockovaci-mista.csv
    - name: Uloz zpravu do repozitare
      run: |-
        git config user.name "Automated"
        git config user.email "actions@users.noreply.github.com"
        git add -A
        timestamp=$(date -u)
        git commit -m "Posledni data: ${timestamp}" || exit 0
        git push
