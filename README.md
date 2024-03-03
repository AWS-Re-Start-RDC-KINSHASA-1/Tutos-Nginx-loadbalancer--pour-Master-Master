# Tutos-Nginx-loadbalancer--pour-Master-Master
Pour faire le loadbalancer nginx pour les instances master master il faut d'abord faire la réplication de la base des données master master si vous ne l'avez pas encore fait alors cliquer sur ce lien https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Replication-Master-Slave-and-Master-Master.git vous trouverez un tutos qui vous explique tout .Une fois que vous avez terminé la réplication de la base de données master master, vous pouvez passer à la configuration du load balancer nginx. Suivez les étapes fournies dans le tutoriel pour mettre en place efficacement votre load balancer et assurer une distribution équilibrée du trafic entre vos instances.
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
    upstream mariadb_cluster {
        server 3.89.200.226:3306;
        server 3.89.206.239:3306;
    }

    server {
        listen 3306;
        proxy_pass mariadb_cluster;
    }
}

```



![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/37274f3a-e7f5-4b12-bb14-b58a0760af78)



l'endroit inquer c'est partie de la configuration , il faut juste remplacer les adresses IP existantes  par les adresses de vos propres serveurs.



Vérifier l'orthographe;


```bash
sudo nginx -t
```

![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/481ba606-e668-401f-a36e-ee68e700b599)


Redemarer nginx


```bash
sudo systemctl restart nginx
```


Test


```bash
mysql -h <Nginx_Public_IP> -u <load_balancer_user> -p
```

On peut aussi tester avec workbench



![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/62f75694-8b5f-4801-8391-9edb56a07ef0)




![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/d0715d07-b9f3-4138-910d-6c2c16a58ed4)




Ensuite cliquer ok;




![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/aff58a81-9e9c-4023-9cda-1ed246739fbc)





Cliquer sur continuer anyway;





![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/700abc7c-ee82-4cce-996b-a054af3e01b1)





![image](https://github.com/AWS-Re-Start-RDC-KINSHASA-1/Tutos-Nginx-loadbalancer--pour-Master-Master/assets/114914329/1ab30a03-fe8b-4679-ba30-f634d829c08c)




Si vous avez cette image , c'est que votre connexion de la base des données master master à partir de loabalancer nginx a marché au cas contraire veuillez revoir votre configuration;
