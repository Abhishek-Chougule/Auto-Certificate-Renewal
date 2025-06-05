# Auto-Certificate-Renewal

File Name : ssl_renew_all_sites.sh

    #!/bin/bash
    
    export PATH=$PATH:/usr/local/bin
    LOG_DIR="/home/user_name/crons_file"
    LOG_FILE="$LOG_DIR/ssl-renew-cron.log"
    DATE=$(date '+%Y-%m-%d %H:%M:%S')
    
    echo "SSL renewal started at $DATE" >> "$LOG_FILE"
    
    echo "Stopping nginx..." >> "$LOG_FILE"
    sudo systemctl stop nginx >> "$LOG_FILE" 2>&1
    
    echo "Running certbot renew..." >> "$LOG_FILE"
    sudo /usr/local/bin/certbot renew >> "$LOG_FILE" 2>&1
    
    echo "Starting nginx..." >> "$LOG_FILE"
    sudo systemctl start nginx >> "$LOG_FILE" 2>&1
    
    DATE_END=$(date '+%Y-%m-%d %H:%M:%S')
    echo "SSL renewal completed at $DATE_END" >> "$LOG_FILE"
    echo "-------------------------------------------" >> "$LOG_FILE"

Cron :  ( sudo crontab -e ) Add Below Line in crontab

    0 6 * * 1 /home/user_name/crons_file/ssl_renew_all_sites.sh >> /home/user_name/crons_file/cron_output.log 2>&1
