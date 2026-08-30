# RAZZAQ
# replace MOHAMMEDRAZZAQ-1468 with your own username
        gh repo create AVIVASHISHTA29 --public --clone
        cd AVIVASHISHTA29
        mkdir -p scripts data .github/workflows
        requests==2.32.3
        beautifulsoup4==4.12.3
        # portrait-only (not needed by the daily workflow):
        pillow
        numpy
        opencv-python
        rembg
        python scripts/prep_photo.py source-photo.jpg
        RAMP = " .`:-=+*cs#%@"   # bright (sparse) -> dark (dense)
        #        ^ leading space clears the background to nothing
        
        python scripts/make_ascii_svg.py   # writes avi-ascii.svg
        python scripts/make_info_card.py   # writes info-card.svg
        python scripts/fetch_contributions.py
        PALETTE = ["#161b22", "#0e4429", "#006d32",
                   "#26a641", "#39d353", "#69f0a0"]
        #          none -> brightest (level 5 is a neon top end)
        <div align="center">

        <h3><code>avi@github ~ $ ./contributions.sh</code></h3>
        <img src="./contrib-heatmap.svg" width="860" />

        <br><br>

        <h3><code>avi@github ~ $ whoami</code></h3>
        <table>
          <tr>
            <td valign="top"><img src="./avi-ascii.svg" width="370" /></td>
            <td valign="top"><img src="./info-card.svg" width="490" /></td>
          </tr>
        </table>

        </div>
         name: Update profile art

        "on":
          schedule:
            - cron: "17 6 * * *"   # ~06:17 UTC daily
          workflow_dispatch: {}
          push:
            branches: [main]

        permissions:
          contents: write

        jobs:
          heatmap:
            runs-on: ubuntu-latest
            steps:
              - uses: actions/checkout@v4
              - uses: actions/setup-python@v5
                with:
                  python-version: "3.11"
              - run: pip install -r scripts/requirements.txt
              - run: python scripts/fetch_contributions.py
              - run: python scripts/render_heatmap_svg.py
              - uses: stefanzweifel/git-auto-commit-action@v5
                with:
                  commit_message: "chore: refresh contribution graph [skip ci]"
                  file_pattern: "data/contributions.json contrib-heatmap.svg

