# Installing Huginn on Heroku (Step-by-step guide for Windows users) 

**Please note:**

  * I'm **not an expert** on any of this and I had **never** previously worked with Heroku, Ruby or Huginn. I'm just posting this here in the hope that it might be helpful for someone.
  * Before you begin make sure you've read the official installation instructions at
https://github.com/cantino/huginn/wiki/Run-Huginn-for-free-on-Heroku

---

If your local computer is a **Windows** machine I recommend that you get VirtualBox and a Ubuntu image (see below) before you attempt to deploy Huginn on Heroku. Unless you know exactly what you're doing, I **strongly discourage** you from doing this directly in Windows.

You will need the following:

  * A [Heroku](https://www.heroku.com/) account
  * [Oracle VM VirtualBox](https://www.virtualbox.org/) **Please note:** If you have antivirus applications like Avira installed on your PC, you might not be able to start up the virtual machine (error code 0x80004005). In this case you'll probably have to downgrade to [VirtualBox Version 4.3.12](https://www.virtualbox.org/wiki/Download_Old_Builds) or earlier.
  * A recent Debian/Ubuntu image. I used the latest Ubuntu image from http://www.osboxes.org/ubuntu/

Here's what you do:

1. Log in to the Heroku website
1. On the Huginn GitHub page click the "Deploy to Heroku" button
1. You'll be taken to Heroku, where you can select a name for your app (don't worry, you can change it later), otherwise leave the default values
1. Wait for the deployment to finish (will take about five minutes)
1. Open VirtualBox and (if you haven't done so already) create a new virtutal machine using the Ubuntu image 
1. Boot up the virtual machine, log in and open a terminal
1. Install Heroku Toolbelt using the `wget` command that can be found at: https://toolbelt.heroku.com/
1. Run `heroku login` and enter your credentials
1. Run `heroku git:clone --app name-of-your-app` (replace `name-of-your-app` with the actual name of your app)
1. Run `cd name-of-your-app` (replace `name-of-your-app` with the actual name of your app)
1. Run the following commands (Note: These may be more or less specific to the Ubuntu image I used and probably aren't necessary for you):
    * `sudo apt-get install bundler`
    * `sudo apt-get install zlib1g-dev` (via http://www.nokogiri.org/tutorials/installing_nokogiri.html)
    * `sudo apt-get install libmysqlclient-dev` (via http://stackoverflow.com/a/3608756)
1. Now run `bundle`. It might not complete successfully at the first attempt but it should always tell you a command to execute at the very bottom of any error message. In such case, run the specified command and afterwards run `bundle` again.
1. After you've ultimately run the setup script at `bin/setup_heroku`, your installation should be ready.

## Some configuration tips
  * If you create agents that are very complex or produce many events on each run, you'll probably have to increase the **maximum agent run time** (defaults to 3 minutes). To do this set the ```DELAYED_JOB_MAX_RUNTIME``` config variable either from the terminal using ```heroku config:set DELAYED_JOB_MAX_RUNTIME=5``` or using the Heroku Dashboard on the web.
  * Also if you want to use event **retention periods** of less the 6 hours (i.e. 1 hour) you need to decrease the value of  ```EVENT_EXPIRATION_CHECK``` (defaults to 6 hours) to 1 hour or less. Run ```heroku config:set EVENT_EXPIRATION_CHECK=1h``` from the terminal or set it using the Heroku Dashboard.
  * If you live anywhere in the world but the US west coast you might want to adjust Huginn's **time zone** setting accordingly. First determine the value by running ```rake time:zones:all``` from the terminal (you might have to install *multi_json* first) and finding the appropriate city name from the list. Then set the ```TIMEZONE``` config variable to that value.