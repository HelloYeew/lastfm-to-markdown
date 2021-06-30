# last.fm to markdown

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 👩🏽‍💻 What you'll need
* A README.md file.
* Last.fm API key
  * Fill [this form](https://www.last.fm/api/account/create) to instantly get one. Requires a last.fm account.
* Set up a GitHub Secret called ```LASTFM_API_KEY``` with the value given by last.fm.
* Also set up a ```LASTFM_USER``` GitHub Secret with the user you'll get the weekly charts for.
* Add a ```<!-- lastfm -->``` tag in your README.md file, with two blank lines below it. The album covers will be placed here.

## Instructions
To use this release, add a ```lastfm.yml``` workflow file to the ```.github/workflows``` folder in your repository with the following code:
```
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
        uses: melipass/lastfm-to-markdown@v1.0.0
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
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

## 🚧 To do
* Allow users to choose the image size and image count for the album covers.
* Feel free to send a PR for anything you believe would be useful.

## 🎵 My weekly last.fm chart (example output)

<!-- lastfm -->
![Paramore - Paramore](https://lastfm.freetls.fastly.net/i/u/64s/bebe11f4ddf3dee473b26c7e2d5c9ff6.png) ![Fishmans - 98.12.28 男達の別れ](https://lastfm.freetls.fastly.net/i/u/64s/f473049c0d8b4dc5cdf70ca773c32ee1.png) ![Paramore - After Laughter](https://lastfm.freetls.fastly.net/i/u/64s/fc4c4f4eb4fa6e9215ecb6705cbb72de.png) ![Fishmans - 宇宙 日本 世田谷](https://lastfm.freetls.fastly.net/i/u/64s/42f09145a2c040959ffe6bbf1a82034c.png) ![Fishmans - Long Season](https://lastfm.freetls.fastly.net/i/u/64s/bff21f34908aa59773d0c3621cb373b0.png) ![The Flaming Lips - Clouds Taste Metallic](https://lastfm.freetls.fastly.net/i/u/64s/3d5fe77ecd5b4863a61cf63cc16392d2.png) ![Paramore - RIOT!](https://lastfm.freetls.fastly.net/i/u/64s/b7a4b3000d0c431fbce299986ac51c48.png) ![Fishmans - 空中キャンプ](https://lastfm.freetls.fastly.net/i/u/64s/534891a8e26aa44f17936987a82f597b.png) ![Paramore - The Summer Tic EP](https://lastfm.freetls.fastly.net/i/u/64s/e01d366ea4064a9aa7cb282c53edbee0.png) ![Paramore - brand new eyes](https://lastfm.freetls.fastly.net/i/u/64s/8935ea2d777c8f2f5f3c7a8f521ea9fb.png) 
