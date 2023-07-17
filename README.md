# Description

Automated Essay Evaluation is an automated essay grading system. It is designed to evaluate and score essays or written responses using artificial intelligence and natural language processing techniques. The system analyzes the content, structure, grammar, and other aspects of the essays to provide objective and consistent scoring. By automating the grading process, it saves time for educators and provides prompt feedback to students. The system aims to enhance efficiency and accuracy in essay assessment, allowing for standardized evaluation while maintaining the human element in education.

# Command

> Set APP
```
export FLASK_APP=app.py
```

> Set Environment
It's can be `development` or `production`.
```
FLASK_ENV=development
```

> Set PORT
```
export FLASK_RUN_PORT=2023
```

> Set HOST
```
export FLASK_RUN_HOST="127.0.0.1"
```

> Run App
```
flask run
```

> Simple Command to Run App
```
flask run -h localhost -p 2023
```

# Deploy Flask App

Create a `systemd service file`. Open a terminal and run the following command to create the file:

```
sudo nano /etc/systemd/system/automatedessay.service
```

In the `automatedessay.service` editor, paste the following content into the file:

```
[Unit]
Description=Flask application for esaiotomatis
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/esaiotomatis
Environment="FLASK_APP=app.py"
ExecStart=/home/ubuntu/esaiotomatis/venv/bin/python -m flask run --host=0.0.0.0 --port=8000
Restart=always

[Install]
WantedBy=multi-user.target
```

Save the file by pressing `Ctrl + X`, then `Y`, and finally `Enter`.

`Enable the service` to start on boot by running the following command:

```
sudo systemctl enable esaiotomatis
```

`Start the service` by running the following command:

```
sudo systemctl start esaiotomatis
```

`Check the service` is running without any errors by running the following command:

```
sudo systemctl status esaiotomatis
```

Install `Nginx` if it's not already installed. 

```
sudo apt update
sudo apt install nginx
```

Create an `Nginx` server block configuration file for your Flask application. Open a new configuration file using a text editor. 

```
sudo nano /etc/nginx/sites-available/esaiotomatis
```

In the configuration file, paste the following configuration, replacing `your_server_ip`` with server's public IP address:

```
server {
    listen 80;
    server_name your_server_ip;

    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Save the configuration file by pressing `Ctrl + X`, then `Y`, and finally `Enter`.

Enable the server block by creating a symbolic link to the `sites-enabled `directory:

```
sudo ln -s /etc/nginx/sites-available/esaiotomatis /etc/nginx/sites-enabled/
```

`Test the Nginx` configuration for any syntax errors:

```
sudo nginx -t
```

If there are `no errors`, `restart Nginx` to apply the changes:

```
sudo systemctl restart nginx
```