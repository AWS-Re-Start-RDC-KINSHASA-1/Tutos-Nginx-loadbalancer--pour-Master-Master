# Tutos-Nginx-loadbalancer--pour-Master-Master
Pour faire le lloadbalancer nginx pour les instances master master il faut d'abord faire la réplication de la base des données master master si vous ne l'avez pas encore fait alors cliquer sur ce lien https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Replication-Master-Slave-and-Master-Master.git vous trouverez un tutos qui vous explique tout .Une fois que vous avez terminé la réplication de la base de données master master, vous pouvez passer à la configuration du load balancer nginx. Suivez les étapes fournies dans le tutoriel pour mettre en place efficacement votre load balancer et assurer une distribution équilibrée du trafic entre vos instances.
Après avoir suivi les étapes, il faut maintenant créer un utilisateur qui te permettra de faire le test avec nginx dans l'une de tes instances;

```bash
CREATE USER 'load_balancer_user'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'load_balancer_user'@'%' IDENTIFIED BY 'your_password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

Ensuite installer nginx;

```bash
sudo apt update
sudo apt install nginx
```

Configurer le fichier nginx pour faire le loadbalancer;

```bash
sudo nano /etc/nginx/nginx.conf
```

Voici le fichier de la configuration nginx;

```bash
stream {
    upstream backend {
        server <EC2_Instance_1_IP>:<Port>;
        server <EC2_Instance_2_IP>:<Port>;
    }

    server {
        listen 3306;
        server_name your_domain.com;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/df90d5dc-4783-421f-9ba2-22ba25a126b2)

l'endroit inquer c'est partie de la configuration , il faut juste remplacer les adresses IP existantes  par les adresses de vos propres serveurs.
