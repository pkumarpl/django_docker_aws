# django_docker_aws
Django project from scratch and deploy on AWS environment

## Instruction of install the docker and other components on AWS server

Lauch an EC2 instance from AWS console with basic coniguration. Then open up Terminal and connect through ssh to the AWS EC2 intance by running:

```bash
ssh ec2-user@PUBLIC_DNS
```

(Replace PUBLIC_DNS with with Public IPv4 DNS link)

In the next step we need on our server are:

1. Git
2. Docker
3. Docker Compose

```bash
sudo yum install git -y
sudo amazon-linux-extras install docker -y
sudo systemctl enable docker.service
sudo systemctl start docker.service
sudo usermod -aG docker ec2-user
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## Copy the project to server
In the next step. clone the project to the server by running:

```bash
git clone git@github.com:pkumarpl/django_docker_aws.git
```

## Run container
Then run the following:

```bash
cp .env.sample .env
```

Edit the.env file, and update the entries to real values:

```bash
DB_NAME=app
DB_USER=admin
DB_PASS=aPassword
SECRET_KEY=aSecretkey
ALLOWED_HOSTS=PUBLIC_DNS
```

Now container can start the project by running:

```bash
docker-compose -f docker-compose-deploy.yml build app

docker-compose -f docker-compose-deploy.yml up -d
```

Check Django admin page running on EC2. In the next step  we need need to create a superuser by running:

```bash
docker-compose -f docker-compose-deploy.yml run --rm app sh -c "python manage.py createsuperuser"
```