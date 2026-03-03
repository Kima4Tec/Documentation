# Linux

## Installer Docker
### Ubuntu’s egen Docker-version:
```
sudo apt update
sudo apt install docker.io -y
```
**Start den:**
```
sudo systemctl enable docker
sudo systemctl start docker
```
**Test:**
```
sudo docker run hello-world
```


## Portainer
#### Installering på Linux
```
docker volume create portainer_data
```

#### Tjek at den kører:
```
docker ps
```

#### Kør portainer
```
docker run -d -p 9000:9000 -p 9443:9443 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
#### Se ip-adresse på Linux. Det er den første.
```
hostname -I
```
#### Hvis min ip-adresse er 192.168.1.123, så tilgås portainer her:
```
http://192.168.1.123:9000
```


## Api forbindelse
Lav ny migration, hvis der er en MS Sql i forvejen, så slet den.
Opdater database.
Lav connectionstring.
### ConnectionString
```
"DefaultConnection": "Host=10.0.1.5;Port=5432;Database=replace;Username=admin;Password=ThePassw0rd"
```
*Host=serverens ip-adresse; Port=porten, hvor postgres ligger; Database: Databasens navn; Username og Password giver sig selv.*

### I program.cs
```
            builder.Services.AddDbContext<AppDbContext>(options =>
                options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```


## Nginx
https://share.google/aimode/GOcsLgLiS16mwSVQC

