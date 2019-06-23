# Hospital Español - Estandar ICD-10

Frontend para búsqueda de diagnosticos según el estandar [ICD-10](https://en.wikipedia.org/wiki/ICD-10)

## Requisitos

1. [yarn](https://yarnpkg.com/en/)
    ```bash
      curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
      echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
      sudo apt update -y && sudo apt install -y yarn
    ```

## Instalación

1. Clonar el repositorio.

```bash
  git clone https://github.com/ortuagustin/diagnosticos-ICD10.git
  cd diagnosticos-ICD10
```

2. Ejecutar `yarn install` para instalar las dependencias
3. Ejecutar `yarn serve` para levantar la aplicación de manera local. Acceder a la URL que se informa en consola
4. Ejecutar `yarn build` para compilar y deployar

<img src="app.png">

## Docker

Para correr la aplicación con *docker*:

### Requisitos:

1. [Docker](https://www.docker.com/)
2. Para no tener que usar `sudo` en los comandos de *docker*:

```bash
  sudo usermod -a -G docker $USER
```

> Es necesario deslogearse para que los cambios tomen efecto.

3. Asegurarse que el servicio *docker* está corriendo:

```bash
  # con systemctl
  sudo systemctl start docker

  # ó, con service
  sudo service docker start
```

### Correr la aplicación

1. Clonar el repositorio.

```bash
  git clone https://github.com/ortuagustin/diagnosticos-ICD10.git
  cd diagnosticos-ICD10
```

2. Crear y correr el contenedor:

```bash
  docker build -t diagnosticos-icd-10 .
  docker run -it -p 8080:80 --rm diagnosticos-icd-10
```

3. La aplicación deberia poder accederse desde [http://localhost:8080/](http://localhost:8080/)
