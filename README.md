Startup Script (start.sh)

#!/bin/bash
# Wait for internet connection
while ! ping -c 1 api.aladhan.com &> /dev/null; do
    sleep 10
done

# Navigate to the application directory
cd /home/mosque/prayer-times
# Start the Flask application
python3 app.py


#Setup Instructions -
Create the directory structure:

bash
mkdir -p prayer-times/templates prayer-times/static/css prayer-times/static/js

#Create each file with the content provided above.
#Make the start.sh script executable:

bash
chmod +x prayer-times/start.sh

#Install required Python packages:
bash
pip3 install flask requests pytz

#Update the systemd service file to use the correct paths:
bash
sudo nano /etc/systemd/system/prayer-times.service
Make sure it has:

[Unit]
Description=Islamic Prayer Times Display
After=network.target
Wants=network.target

[Service]
Type=simple
User=mosque
Group=mosque
WorkingDirectory=/home/mosque/prayer-times
ExecStart=/home/mosque/prayer-times/start.sh
Restart=always
RestartSec=10
Environment=PYTHONUNBUFFERED=1

[Install]
WantedBy=multi-user.target

Reload and restart the service:
bash
sudo systemctl daemon-reload
sudo systemctl enable prayer-times.service
sudo systemctl start prayer-times.service
