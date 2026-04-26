# Deploying NammaShanti Bot to Google Compute Engine (GCE)

This is the easiest way to keep `bot.py` running 24/7 in the cloud without modifying your code to use Webhooks or Docker.

## Step 1: Create the Virtual Machine
1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Make sure your project `nammashanti-20b1e` is selected in the top dropdown.
3. Open the left menu (hamburger icon) -> **Compute Engine** -> **VM instances**.
4. If prompted, click **Enable** to enable the Compute Engine API.
5. Click **Create Instance**.
6. Set the following options:
   * **Name:** `nammashanti-bot-vm`
   * **Region:** `asia-south1` (Mumbai) or any region near you.
   * **Machine configuration:** Choose **General-purpose** -> **E2** -> **e2-micro** (This is free-tier eligible).
   * **Boot disk:** Leave the default (Debian GNU/Linux).
   * **Firewall:** Check both **Allow HTTP traffic** and **Allow HTTPS traffic**.
7. Click **Create**. Wait a minute for the VM to start up.

## Step 2: Connect to the VM
1. Once the VM is running, you will see an **SSH** button next to it in the list.
2. Click the **SSH** button. A browser-based terminal window will open connected to your new Linux server.

## Step 3: Install Required Software
Run these commands in the SSH terminal to install Python and Git:
```bash
sudo apt update
sudo apt install -y python3 python3-pip python3-venv git
```

## Step 4: Transfer Your Code
Since your code is likely on your local Mac, the easiest way to get it onto the VM is by cloning it from GitHub. If you haven't pushed your code to GitHub, you can upload the files directly using the "Upload File" button (the upward arrow icon) in the top-right of the SSH window.

If uploading manually, upload the following files from your `backend` folder:
* `bot.py`
* `engine.py`
* `requirements.txt`
* `.env`
* `firebase-service-account.json`

Make a directory for them and move them in:
```bash
mkdir backend
mv bot.py engine.py requirements.txt .env firebase-service-account.json backend/
cd backend
```

## Step 5: Setup Python Environment
Inside the `backend` folder on the VM, run:
```bash
# Create a virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Step 6: Run the Bot 24/7
If you just run `python bot.py`, it will stop when you close the SSH window. To keep it running permanently in the background, use `nohup`:

```bash
nohup python bot.py > bot.log 2>&1 &
```

**That's it!** 
* The bot is now running in the background. 
* You can safely close the SSH browser window.
* To view the bot's live output at any time, SSH back in and run: `tail -f backend/bot.log`
* To stop the bot, run: `pkill -f "python bot.py"`

Your Telegram bot is now hosted on the cloud!
