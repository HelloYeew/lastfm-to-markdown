# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><img src="https://lastfm.freetls.fastly.net/i/u/64s/d9c0e65ca20d086af5a087a9d1b1b99b.jpg" title="Gufi - Corazón d' Roto"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/e415585c828fc8a5bf0eabdb94b84893.jpg" title="Guerilla Toss - Twisted Crystal"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/1f64bb6f272dee7ffda769bd20304342.png" title="The Gun Club - Fire of Love"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/73094ab74581cb12e374b61180b69579.jpg" title="Guerilla Toss - Eraser Stargazer"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/67ebb2797eb7119146d6df292cb1449d.jpg" title="Guerilla Toss - GT Ultra"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/221fdf7c137879cdca2a79a375d254f8.jpg" title="Los Prisioneros - Corazones"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/04fbb96494d648d6c256193032883779.jpg" title="Los Tres - Se Remata el Siglo"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/fd2ad617a6182858699afaa09e19d163.jpg" title="Guerilla Toss - Gay Disco"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/5fb0ed8c165249b4ab6541872242abd3.png" title="Grouper - Cover the Windows and the Walls"> </p>

          
## 👩🏽‍💻 What you'll need
* A README.md file.
* Last.fm API key
  * Fill [this form](https://www.last.fm/api/account/create) to instantly get one. Requires a last.fm account.
* Set up a GitHub Secret called ```LASTFM_API_KEY``` with the value given by last.fm.
* Also set up a ```LASTFM_USER``` GitHub Secret with the user you'll get the weekly charts for.
* Add a ```<!-- lastfm -->``` tag in your README.md file, with two blank lines below it. The album covers will be placed here.

## Instructions
To use this release, add a ```lastfm.yml``` workflow file to the ```.github/workflows``` folder in your repository with the following code:
```diff
name: lastfm-to-markdown

on:
  schedule:
    - cron: '2 0 * * *'
  workflow_dispatch:

jobs:
  lastfm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: lastfm to markdown
        uses: melipass/lastfm-to-markdown@v1.3
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         IMAGE_COUNT: 6 # Optional. Defaults to 10. Feel free to remove this line if you want.
      - name: commit changes
        continue-on-error: true
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add -A
          git commit -m "Updated last.fm's weekly chart" -a

      - name: push changes
        continue-on-error: true
        uses: ad-m/github-push-action@v0.6.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}\
          branch: main
```
The cron job is scheduled to run once a day because Last.fm's API updates weekly chart data daily at 00:00, it's useless to make more than 1 request per day because you'll get the same information back every time. You can manually run the workflow in case Last.fm's API was down at the time, going to the Actions tab in your repository.

## 🚧 To do
* Allow users to choose the image size for the album covers.
* Feel free to open an issue or send a pull request for anything you believe would be useful.
