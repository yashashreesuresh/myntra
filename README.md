### Myntra Gadgets Scraper

Selenium is required to scrape javascript websites. Using `scrapy-selenium` middleware. Refer [this](https://github.com/clemfromspace/scrapy-selenium) doc.

Uses selenium to automate the task to navigating to all the pages in gadgets section of Myntra and scrapes the details of each product.

#### Steps to deploy to scrapyd

1. Install scrapy daemon by executing `pip3 install scrapyd`.
2. Install scrapy-client byexecuting `pip3 install git+https://github.com/iamumairayub/scrapyd-client.git --upgrade`.
3. Execute `scrapyd` in one terminal.
4. Change `[deploy]` to `[deploy:local] or [deploy:<str>]` in scrapy.cfg.
5. Uncomment the `url = http://localhost:6800/` under `[deploy]` in scrapy.cfg.
5. Execute `scrapyd-deploy local` in another terminal.
6. Execute `curl http://localhost:6800/schedule.json -d project=myntra -d spider=gadgets` to start the spider ececution.
7. Execute step 5 and step 6 whenever a change is made in the project to update the same in scrapy daemon.
8. Execute `curl http://localhost:6800/cancel.json -d project=myntra -d job=<job_id>` to stop the running spider.

#### Steps to deploy to Heroku

1. Install herokuify_scrapyd by executing `pip3 install herokuify_scrapyd`.
2. Create requirement.txt file by executing `pip3 freeze > requirements.txt`.
3. Create Procfile and runtime.txt file.
4. Login to Heroku: `heroku login -i`.
5. Create heroku app by executing `heroku create`.
6. Copy the url (eg, `https://limitless-waters-77333.herokuapp.com/`) returned and paste it under [deploy] section in scrapy.cfg.
7. Add `[scrapyd]` section in scrapy.cfg.
8. Add heroku to remote `heroku git:remote -a <app_name>`, for example, `heroku git:remote -a limitless-waters-77333`.
9. Push the code to heroku. `git init` -> `git add .` -> `git commit -m <commit_message>` -> `git push heroku master`.
10. Deploy the app. `scrapyd-deploy local` -> `curl https://limitless-waters-77333.herokuapp.com/schedule.json -d project=myntra -d spider=gadgets`.

NOTE: Configure the settings.py file as [required](https://stackoverflow.com/questions/61787960/using-scrapy-selenium-middleware-on-heroku) for scrapy-selenium middleware.
```
#SELENIUM
import os

SELENIUM_DRIVER_NAME = 'chrome'
SELENIUM_DRIVER_EXECUTABLE_PATH = os.environ.get("CHROMEDRIVER_PATH")
SELENIUM_DRIVER_ARGUMENTS=["--headless", "--no-sandbox", "--disable-gpu", "--disable-dev-shm-usage"]
SELENIUM_BROWSER_EXECUTABLE_PATH = os.environ.get("GOOGLE_CHROME_BIN")
```

NOTE: Configure heroku app forselenium in portal as specified [here](https://romik-kelesh.medium.com/how-to-deploy-a-python-web-scraper-with-selenium-on-heroku-1459cb3ac76c).