# -------------------------------------------------------------------------------------------------
#  Repository Helper to catch parent Makefile recipes
# -------------------------------------------------------------------------------------------------
%:
	@$(MAKE) -C .. $@

# -------------------------------------------------------------------------------------------------
#  Repository Helper for application recipes
# -------------------------------------------------------------------------------------------------
.PHONY: go-help go-run go-build go-test go-migrate-up go-migrate-down

# APP="cmd/api/main.go"
# APP_BUILD="bin/api cmd/api/main.go"
APP=main.go
APP_BUILD=$(APP)
go-help: ## Show this help message
	@echo 'Usage: make [target]'
	@echo ''
	@echo 'Available targets:'
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "  %-15s %s\n", $$1, $$2}' $(MAKEFILE_LIST)

go-run: ## Run the application
	go run $(APP)

go-build: ## Build the application
	go build -o $(APP_BUILD)

go-test: ## Run tests
	go test -v ./...

go-clean: ## Clean build artifacts
	rm -rf bin/
	rm -rf tmp/

go-install: ## Install dependencies
	go mod download
	go mod tidy
