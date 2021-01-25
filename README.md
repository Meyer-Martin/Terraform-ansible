# Fichier de Terraform & ansible

J'ai choisis le TP n°2

TP 2


Création d'un serveur Jenkins et de deux pipelines
Techno : AWS, Terraform, Ansible, Docker, Jenkins
Enoncés :
    Installation de Jenkins
    -----------------------
    - Code Terraform doit contenir :
        - Instance sur le vpc_jenkins
        - Création des security groups associés :
            - port 22 pour le SSH 
            - port 8080 pour jenkins
            - port 80 pour les serveurs Web
        - Utilisation de la clé SSH tp_dev_ynov
    - Code Ansible doit contenir :
        - Installation de docker
        - Utilisateur Ubuntu doit avoir accès à docker (dans le même groupe)
        - Récupération du repo git cours_ynov
    - Build de l'image docker disponible sur le repo (voir dossier Jenkins)
    - L'image doit s'appeler jenkins_master
    - Création d'un conteneur jenkins_master
        - le conteneur jenkins_master doit pouvoir utiliser docker via jenkins
    - Installation des plugins Docker, Blue Ocean, CloudBees AWS Credentials via l'interface de Jenkins
    - Configuration de docker dans Jenkins
        - unix://var/run/docker.sock
    - Ajout des credentials AWS (access_key et secret_key) dans Jenkins
    
    Création pipeline
    -----------------
    - Une pipeline de création d'instances via Terraform
        - Code Terraform doit contenir :
            - Création d'une ou plusieurs instances (count)
            - Dans le VPC_jenkins
            - Utilisation de la clé SSH tp_dev_ynov
            - Utilisation du backend S3 => tp-terraform-ynov
        - Code Jenkinsfile doit contenir :
            - Utilisation d'un conteneur Docker avec l'image hashicorp/terraform:light
            - Le conteneur doit avoir comme argument : entrypoint=""
            - La pipeline doit contenir 4 steps :
                - Terraform init
                - Terraform plan
                - Terraform apply
                - Terraform output (Récupération de l'IP de l'instance)
            - Les credentials ne doivent pas être en clair
    - Une pipeline de configuration d'instances via Ansible
       - Code Ansible doit contenir :
            - Installation d'un serveur Web NGINX et de GIT
            - Récupération de votre code HTML depuis votre repo
                git clone https:<url_repo>/<nom_repo>.git /var/www/html/
       - Code Jenkinsfile doit contenir :
            - Utilisation de votre Dockerfile (Installation d'Ansible + ajout de la clé SSH)
            - La pipeline doit contenir 1 step:
                - Lancement du playbook
    - Vos deux pipelines doivent se déclencher à chaque changement de votre code Terraform et code HTML de votre repo
    - Règle de nommage :
      - instances :
        - instance_jenkins_server_<nom>
        - instance_terraform_<nom>
      - security groups:
        - security_group_jenkins_<nom>
