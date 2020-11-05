# Blynk local server setup & configuration
Configuration Steps for getting Blynk Server to work locally on RPi Zero W

1. Create folder to contain all server files. (eg. /Blynk)

2. Download Server File from location: https://github.com/blynkkk/blynk-server/releases

3. Install Java 8: openjdk-8-jdk & openjdk-8-jre

4. Create Data Folder within Blynk Folder (eg. /Blynk/BlynkData)

5. Create self-signed OpenSSL certificates:

    a. Run:   openssl req -x509 -nodes -days 1825 -newkey rsa:2048 -keyout server.key -out server.crt <br/>
    b. Afterwards, run: openssl pkcs8 -topk8 -v1 PBE-SHA1-2DES -in server.key -out server.enc.key
    
6. Pull server.config and mail.config from locations: <br/>
      https://github.com/blynkkk/blynk-server/blob/master/server/core/src/main/resources/server.properties <br/>
      https://github.com/blynkkk/blynk-server/blob/master/server/notifications/email/src/main/resources/mail.properties<br/>
      
7. In server.properties, add generated *.crt and *.enc.key paths to \
    server.ssl.cert = /.../... \
    server.ssl.key=/.../... \
    server.ssl.key.pass=/... 

8. In server.properties, configure server host with local server IP.

9. In order to configure server restart on reboot, edit crontab (command crontab -e) and add the following part at the end.
      @reboot sleep 120 && java -jar /.../.../(location of server file).jar -dataFolder /.../.../(location of data folder) -serverConfig /.../.../(location of server.properties) -mailConfig /.../.../(location of mail.properties)
      
10. Edit Google Account preferences to allow less secure apps. Ideally create a separate google account for this purpose.
