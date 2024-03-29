GIT_HASH = $(shell git describe --tags --dirty --always)

.PHONY: all run lint fmt test build clean help

all: clean test build run ## clean dev env, run all linting and tests, build binary and run server

run: ## Run the project
	./bin/project
	
test: lint fmt unit ## Execute all linting and tests 

unit: ## Execute unit tests 
	go test ./... -coverprofile cover.out
	
lint:
	go vet ./...

fmt:
	go fmt ./...

build: test ## Build project binary
	CGO_ENABLED=1 GOOS=linux GOARCH=amd64 GO111MODULE=on GOSUMDB=off go build -o bin/project main.go

docker: ## Build project dockerfile
	docker build . -f image/Dockerfile
	
clean: ## Clean the development environment
	rm -rf bin cover.out *.log *.db

help: ## Show help messages
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'