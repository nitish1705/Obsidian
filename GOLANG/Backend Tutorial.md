
we create the basic folder structure!

![[Pasted image 20260629203341.png]]


Now inside main.go we write the following basic code

```Go
package main

import (
	"log"
	"net/http"

	"gobackend/internal/handlers"
)

func main() {

	http.HandleFunc("/health", handlers.HealthCheck)


	log.Println("Server starting on :8080")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```

The below is inside the basic endpoint /health which is inside the health.go file

```go
package handlers

import (
	"net/http"
)

// HealthCheck verifies the API is up and running.
func HealthCheck(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Write([]byte("OK"))
}
```


The go.mod is created by itself using the command : 
```bash
go mod init gobackend
```

the "gobackend" is the module name which will be created on which acts as the root directory name which will be used to import pages into the main

Okay so for next,  we will be having middleware which logs the actions that is performed by the HandlerFunc

```go
package middleware

import (
	"net/http"
	"log/slog"
	"time"
)

func Logger(next http.HandlerFunc) http.HandlerFunc{
	return func(w http.ResponseWriter, r *http.Request){
		start := time.Now()

		next(w, r);
		slog.Info("incoming request",
			"method", r.Method,
			"path", r.URL.Path,
			"duration", time.Since(start).String(),
		)
	}
}
```

The code will log into the terminal in a order basis of what it is specified in the log

Now, We would like to specify that certain function is for method "GET" or "POST", to do that we simply just mention "GET / health... " while calling the certain handlerfunc

```go
package main

import (
	"log"
	"net/http"

	"gobackend/internal/handlers"
	"gobackend/internal/middleware"
)

func main() {


	http.HandleFunc("GET /health", middleware.Logger(handlers.HealthCheck))

	log.Println("Server starting on :8080")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```

Now, We should be good! This is the very basic backend a person can create! Let's talk about talking to database itself, for this we will need postegre SQL, For industry standards, we will use docker to run our backend and our backend correctly. For that we would need to files to be created at the root directory, "DockerFile", "docker-compose.yml". 

Before that we would need to install a certain git file using the go get command. which is

```bash
go get github.com/jackc/pgx/v5/pgxpool
```

Now the two files would be DockerFile and docker-compose.yml files respectively

```DockerFile
# Stage 1: Build the Go binary
FROM golang:1.26-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the application code
COPY . .

# Build the executable binary
RUN go build -o main ./cmd/api/main.go

# Stage 2: Create a tiny final image
FROM alpine:latest

WORKDIR /app

# Copy only the compiled binary from the builder stage
COPY --from=builder /app/main .

# Expose the port the app runs on
EXPOSE 8080

# Command to run the executable
CMD ["./main"]
```

```yml
version: '3.8'

services:
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: my_database
    ports:
      - "5432:5432"

  api:
    build: .
    ports:
      - "8080:8080"
    environment:
      DB_DSN: postgres://myuser:mypassword@db:5432/my_database
    depends_on:
      - db
```

This is it! We run the docker compose up --build to recompile everything and the next time we start the backend and database, we simpley just type docker compose up and that's more than enough!