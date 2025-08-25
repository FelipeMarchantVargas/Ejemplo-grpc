# Proyecto Base gRPC en Go

Este repositorio contiene una implementación minimalista de un cliente y un servidor que se comunican a través de gRPC, utilizando Go. Está diseñado para servir como un punto de partida claro y funcional para proyectos de sistemas distribuidos.

## Tabla de Contenidos

1. [Requisitos Previos](#1-requisitos-previos-instalación-del-entorno)
2. [Configuración del Proyecto](#2-configuración-del-proyecto)
3. [Ejecución](#3-ejecución-del-proyecto)
4. [Flujo de Desarrollo](#4-flujo-de-desarrollo-importante)

---

### 1. Requisitos Previos (Instalación del Entorno)

Antes de clonar el proyecto, asegúrate de tener el siguiente entorno configurado en tu sistema **Pop!\_OS**.

#### 1.1. Instalar Go

El lenguaje de programación principal del proyecto.

```bash
# Actualiza los repositorios e instala Go
sudo apt update
sudo apt install golang-go

# Verifica la instalación
go version
```

#### 1.2. Instalar Protocol Buffers Compiler (`protoc`)

La herramienta que compila nuestros archivos `.proto` en código Go.

```bash
# Instala el paquete del compilador
sudo apt install protobuf-compiler

# Verifica la instalación
protoc --version
```

#### 1.3. Instalar los Plugins de `protoc` para Go

Estos son los "ayudantes" que `protoc` necesita para generar el código específico de Go y gRPC.

```bash
# Instala el generador para Go
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

# Instala el generador para gRPC en Go
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

#### 1.4. Configurar las Variables de Entorno (Paso Crucial)

Para que tu terminal pueda encontrar `go` y los plugins que acabas de instalar, necesitas añadir sus ubicaciones a la variable de entorno `PATH`.

```bash
# Ejecuta este comando para añadir la configuración correcta a tu archivo .bashrc
echo '
# --- Configuración de Go ---
export GOROOT=/usr/lib/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> ~/.bashrc

# Aplica los cambios a tu sesión actual de la terminal
source ~/.bashrc
```

**Nota:** Es recomendable cerrar y volver a abrir la terminal después de este paso para asegurar que todos los cambios se han cargado correctamente.

---

### 2. Configuración del Proyecto

Una vez que el entorno está listo, puedes clonar y configurar el proyecto.

#### 2.1. Clonar el Repositorio

```bash
# Reemplaza la URL con la de tu repositorio
git clone https://github.com/tu-usuario/grpc-atraco.git

# Navega al directorio del proyecto
cd grpc-atraco
```

#### 2.2. Descargar las Dependencias

Este comando leerá el archivo `go.mod` y descargará las librerías necesarias (como `grpc-go`).

````bash
# Desde la raíz del proyecto
go mod tidy```

---

### 3. Ejecución del Proyecto

Para correr la aplicación, necesitas dos terminales, ambas ubicadas en la raíz del proyecto.

#### Terminal 1: Iniciar el Servidor

El servidor se quedará escuchando peticiones de los clientes.

```bash
go run server/server.go
````

_Deberías ver un mensaje como: `Servidor escuchando en [::]:50051`_

#### Terminal 2: Iniciar el Cliente

El cliente se conectará al servidor, enviará una petición y mostrará la respuesta.

```bash
go run client/client.go
```

_Deberías ver la respuesta del servidor: `Respuesta del servidor: Hola, Mundo!`_

---

### 4. Flujo de Desarrollo (Importante)

Si en algún momento modificas el archivo `proto/greeter.proto` (por ejemplo, para añadir una nueva función o cambiar un mensaje), **es obligatorio regenerar el código Go**.

Para hacerlo, ejecuta el siguiente comando desde la **raíz del proyecto**:

```bash
protoc --go_out=. --go-grpc_out=. proto/greeter.proto
```

Esto actualizará los archivos `greeter.pb.go` y `greeter_grpc.pb.go` con tus nuevos cambios.
