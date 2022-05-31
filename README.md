# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/The+Go-Betweens/Liberty+Belle+and+the+Black+Diamond+Express"><img src="https://lastfm.freetls.fastly.net/i/u/64s/9198d7af00f040cc9c6e44de76c0a5bd.png" title="The Go-Betweens - Liberty Belle and the Black Diamond Express"></a> <a href="https://www.last.fm/music/Charles+Mingus/Mingus+Ah+Um"><img src="https://lastfm.freetls.fastly.net/i/u/64s/bb95a707719645438c999b700d4d1633.png" title="Charles Mingus - Mingus Ah Um"></a> <a href="https://www.last.fm/music/The+Pooh+Sticks/The+Great+White+Wonder"><img src="https://lastfm.freetls.fastly.net/i/u/64s/fd7a9da0dde4b5ca4dc69395a908bce6.jpg" title="The Pooh Sticks - The Great White Wonder"></a> <a href="https://www.last.fm/music/Everyone+Asked+About+You/Everyone+Asked+About+You"><img src="https://lastfm.freetls.fastly.net/i/u/64s/249a8c32494ee395f54116533533755e.png" title="Everyone Asked About You - Everyone Asked About You"></a> <a href="https://www.last.fm/music/Everyone+Asked+About+You/sometimes+memory+fails+me+sometimes"><img src="https://lastfm.freetls.fastly.net/i/u/64s/cb6c3feb89604115c12ffdc6d45c85f1.jpg" title="Everyone Asked About You - sometimes memory fails me sometimes"></a> <a href="https://www.last.fm/music/Everyone+Asked+About+You/The+Boston+To+Little+Rock+Connection+Split"><img src="https://lastfm.freetls.fastly.net/i/u/64s/e46b70a4b414833f0731a6b6a644f875.jpg" title="Everyone Asked About You - The Boston To Little Rock Connection Split"></a> <a href="https://www.last.fm/music/Hildur+Gu%C3%B0nad%C3%B3ttir/Leyf%C3%B0u+Lj%C3%B3sinu"><img src="https://lastfm.freetls.fastly.net/i/u/64s/dbbbd80190a9814a00c30673ccfa233e.jpg" title="Hildur Guðnadóttir - Leyfðu Ljósinu"></a> <a href="https://www.last.fm/music/My+Chemical+Romance/The+Foundations+of+Decay+-+Single"><img src="https://lastfm.freetls.fastly.net/i/u/64s/269349eef88cb86a2c55407b3e77e710.jpg" title="My Chemical Romance - The Foundations of Decay - Single"></a> <a href="https://www.last.fm/music/The+Shyness+Clinic/The+Boston+To+Little+Rock+Connection+Split"><img src="https://lastfm.freetls.fastly.net/i/u/64s/fc545ff866b011f9fe74f69bed060c43.png" title="The Shyness Clinic - The Boston To Little Rock Connection Split"></a> </p>

          
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
        uses: melipass/lastfm-to-markdown@v1.3.1
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         INCLUDE_LINK: true # Optional. Defaults is false. If you want to include the link to the album page, set this to true.
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
