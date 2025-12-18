# .NET CLI Guide

A comprehensive guide to working with the .NET Command Line Interface (CLI) for building, running, testing, and managing .NET applications.

## Table of Contents
- [Getting Started](#getting-started)
- [Project Management](#project-management)
- [Building and Running](#building-and-running)
- [Package Management (NuGet)](#package-management-nuget)
- [Testing](#testing)
- [Publishing and Deployment](#publishing-and-deployment)
- [Solution Management](#solution-management)
- [Tool Management](#tool-management)
- [Common Workflows](#common-workflows)
- [Configuration and Settings](#configuration-and-settings)
- [Advanced Scenarios](#advanced-scenarios)
- [Tips and Best Practices](#tips-and-best-practices)

---

## Getting Started

### Check Installation and Version
```bash
# Check .NET SDK version
dotnet --version

# List all installed SDKs
dotnet --list-sdks

# List all installed runtimes
dotnet --list-runtimes

# Display .NET information
dotnet --info
```

### Get Help
```bash
# General help
dotnet --help

# Help for a specific command
dotnet new --help
dotnet build --help
dotnet run --help
```

---

## Project Management

### Creating New Projects

```bash
# List available templates
dotnet new list
dotnet new --list

# Create a console application
dotnet new console -n MyConsoleApp
dotnet new console -o MyConsoleApp

# Create a web API
dotnet new webapi -n MyApi

# Create an ASP.NET Core MVC app
dotnet new mvc -n MyWebApp

# Create a class library
dotnet new classlib -n MyLibrary

# Create a Blazor app
dotnet new blazorserver -n MyBlazorApp
dotnet new blazorwasm -n MyBlazorWasmApp

# Create a test project
dotnet new xunit -n MyTests
dotnet new nunit -n MyTests
dotnet new mstest -n MyTests

# Create a worker service
dotnet new worker -n MyBackgroundService

# Create a Razor Pages app
dotnet new razor -n MyRazorApp

# Create with specific framework version
dotnet new console -n MyApp -f net8.0
dotnet new webapi -n MyApi -f net7.0
```

### Project File Operations

```bash
# Add a reference to another project
dotnet add reference ../MyLibrary/MyLibrary.csproj

# Add multiple references
dotnet add reference ../Lib1/Lib1.csproj ../Lib2/Lib2.csproj

# Remove a project reference
dotnet remove reference ../MyLibrary/MyLibrary.csproj

# List project references
dotnet list reference
```

---

## Building and Running

### Building Projects

```bash
# Build the project
dotnet build

# Build in Release configuration
dotnet build -c Release
dotnet build --configuration Release

# Build with specific framework
dotnet build -f net8.0

# Build without restoring packages
dotnet build --no-restore

# Build with verbose output
dotnet build -v detailed
dotnet build --verbosity normal

# Clean build output
dotnet clean

# Clean and rebuild
dotnet clean && dotnet build
```

### Running Applications

```bash
# Run the project
dotnet run

# Run with specific configuration
dotnet run -c Release

# Run with command-line arguments
dotnet run -- arg1 arg2 arg3
dotnet run --project ./MyApp/MyApp.csproj -- --verbose

# Run a specific DLL
dotnet MyApp.dll

# Run with environment variables
dotnet run --environment Production
ASPNETCORE_ENVIRONMENT=Production dotnet run

# Watch mode (auto-restart on file changes)
dotnet watch run
dotnet watch test
dotnet watch build
```

### Restore Dependencies

```bash
# Restore NuGet packages
dotnet restore

# Restore for specific runtime
dotnet restore -r linux-x64

# Force re-evaluation of all dependencies
dotnet restore --force

# Restore with specific package source
dotnet restore --source https://api.nuget.org/v3/index.json
```

---

## Package Management (NuGet)

### Adding Packages

```bash
# Add a NuGet package
dotnet add package Newtonsoft.Json

# Add a specific version
dotnet add package Newtonsoft.Json --version 13.0.3

# Add the latest prerelease version
dotnet add package Microsoft.EntityFrameworkCore --prerelease

# Add from a specific source
dotnet add package MyPackage --source https://myget.org/feed
```

### Removing and Listing Packages

```bash
# Remove a package
dotnet remove package Newtonsoft.Json

# List installed packages
dotnet list package

# List outdated packages
dotnet list package --outdated

# List deprecated packages
dotnet list package --deprecated

# List vulnerable packages
dotnet list package --vulnerable

# List packages with version details
dotnet list package --include-transitive
```

### Updating Packages

```bash
# Update all packages (modify .csproj manually or use tools)
# Note: dotnet CLI doesn't have built-in update command
# Use Visual Studio or third-party tools like dotnet-outdated

# Install dotnet-outdated global tool
dotnet tool install --global dotnet-outdated-tool

# Update packages using the tool
dotnet outdated --upgrade
```

---

## Testing

### Running Tests

```bash
# Run all tests
dotnet test

# Run tests in Release mode
dotnet test -c Release

# Run tests with verbose output
dotnet test -v detailed

# Run tests without building
dotnet test --no-build

# Run tests with code coverage
dotnet test --collect:"XPlat Code Coverage"

# Run specific test by filter
dotnet test --filter "FullyQualifiedName~MyNamespace.MyTestClass"
dotnet test --filter "Category=Unit"
dotnet test --filter "Priority=1"

# Run tests with logger options
dotnet test --logger "console;verbosity=detailed"
dotnet test --logger "trx"
dotnet test --logger "html"
```

### Advanced Testing

```bash
# Run tests in parallel (default)
dotnet test --parallel

# Run tests sequentially
dotnet test -- RunConfiguration.MaxCpuCount=1

# Set test timeout
dotnet test -- NUnit.TestTimeout=5000

# Run tests with specific settings file
dotnet test --settings test.runsettings

# List all tests without running them
dotnet test --list-tests

# Run tests and continue on failure
dotnet test --no-restore --no-build || true
```

---

## Publishing and Deployment

### Publishing Applications

```bash
# Publish for current platform
dotnet publish

# Publish in Release mode
dotnet publish -c Release

# Publish to specific folder
dotnet publish -o ./publish

# Publish for specific runtime (self-contained)
dotnet publish -r win-x64 --self-contained
dotnet publish -r linux-x64 --self-contained
dotnet publish -r osx-x64 --self-contained

# Publish as framework-dependent
dotnet publish -r win-x64 --self-contained false

# Publish with single file output
dotnet publish -r win-x64 --self-contained -p:PublishSingleFile=true

# Publish with trimming (reduce size)
dotnet publish -c Release -r linux-x64 --self-contained \
  -p:PublishTrimmed=true -p:PublishSingleFile=true

# Publish without restore or build
dotnet publish --no-restore --no-build
```

### Publishing Web Applications

```bash
# Publish web app for IIS
dotnet publish -c Release -o ./publish

# Publish for Docker (Linux)
dotnet publish -c Release -r linux-x64 --self-contained false

# Publish with ReadyToRun compilation
dotnet publish -c Release -r win-x64 \
  -p:PublishReadyToRun=true
```

### Runtime Identifiers (Common)

```bash
# Windows
win-x64, win-x86, win-arm64

# Linux
linux-x64, linux-arm, linux-arm64

# macOS
osx-x64, osx-arm64

# See full list
# https://learn.microsoft.com/en-us/dotnet/core/rid-catalog
```

---

## Solution Management

### Working with Solutions

```bash
# Create a new solution
dotnet new sln -n MySolution

# Add projects to solution
dotnet sln add MyApp/MyApp.csproj
dotnet sln add MyLibrary/MyLibrary.csproj MyTests/MyTests.csproj

# Remove project from solution
dotnet sln remove MyTests/MyTests.csproj

# List projects in solution
dotnet sln list

# Build entire solution
dotnet build MySolution.sln

# Test entire solution
dotnet test MySolution.sln
```

### Typical Solution Structure Workflow

```bash
# Create solution and projects
dotnet new sln -n MyApp
dotnet new webapi -n MyApp.Api
dotnet new classlib -n MyApp.Core
dotnet new xunit -n MyApp.Tests

# Add all projects to solution
dotnet sln add MyApp.Api/MyApp.Api.csproj
dotnet sln add MyApp.Core/MyApp.Core.csproj
dotnet sln add MyApp.Tests/MyApp.Tests.csproj

# Add project references
cd MyApp.Api
dotnet add reference ../MyApp.Core/MyApp.Core.csproj
cd ../MyApp.Tests
dotnet add reference ../MyApp.Core/MyApp.Core.csproj
dotnet add package Microsoft.NET.Test.Sdk
dotnet add package xunit
dotnet add package xunit.runner.visualstudio

# Build the solution
cd ..
dotnet build
```

---

## Tool Management

### Global Tools

```bash
# Install a global tool
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-outdated-tool
dotnet tool install --global dotnet-format

# List installed global tools
dotnet tool list --global

# Update a global tool
dotnet tool update --global dotnet-ef

# Uninstall a global tool
dotnet tool uninstall --global dotnet-ef

# Run a global tool
dotnet ef --help
dotnet format --help
```

### Local Tools

```bash
# Create tool manifest
dotnet new tool-manifest

# Install a local tool
dotnet tool install dotnet-ef
dotnet tool install dotnet-format

# List local tools
dotnet tool list

# Run a local tool
dotnet tool run dotnet-ef
dotnet ef  # Shorthand

# Restore tools from manifest
dotnet tool restore

# Update local tool
dotnet tool update dotnet-ef
```

### Popular .NET Tools

```bash
# Entity Framework Core tools
dotnet tool install --global dotnet-ef

# Format code
dotnet tool install --global dotnet-format

# Check for outdated packages
dotnet tool install --global dotnet-outdated-tool

# User secrets management (built-in)
dotnet user-secrets --help

# Development certificates (built-in)
dotnet dev-certs https --trust

# Database migrations
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

## Common Workflows

### Starting a New Web API Project

```bash
# Create and set up new Web API
dotnet new webapi -n MyApi
cd MyApi

# Add common packages
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Swashbuckle.AspNetCore

# Install EF Core tools
dotnet tool install --global dotnet-ef

# Run the application
dotnet run

# Run in watch mode for development
dotnet watch run
```

### Working with Entity Framework Core

```bash
# Install EF Core tools
dotnet tool install --global dotnet-ef

# Add EF Core packages
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Design

# Create a migration
dotnet ef migrations add InitialCreate

# Update database
dotnet ef database update

# Rollback migration
dotnet ef database update PreviousMigrationName

# Remove last migration
dotnet ef migrations remove

# List migrations
dotnet ef migrations list

# Generate SQL script
dotnet ef migrations script

# Drop database
dotnet ef database drop

# Get DbContext info
dotnet ef dbcontext info
dotnet ef dbcontext list
```

### Managing User Secrets (Development)

```bash
# Initialize user secrets
dotnet user-secrets init

# Set a secret
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=..."
dotnet user-secrets set "ApiKey" "my-secret-api-key"

# List all secrets
dotnet user-secrets list

# Remove a secret
dotnet user-secrets remove "ApiKey"

# Clear all secrets
dotnet user-secrets clear
```

### Working with Development Certificates

```bash
# Trust the HTTPS development certificate
dotnet dev-certs https --trust

# Export certificate
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p password

# Clean certificates
dotnet dev-certs https --clean

# Check certificate
dotnet dev-certs https --check
```

---

## Configuration and Settings

### Setting the Target Framework

```xml
<!-- In .csproj file -->
<PropertyGroup>
  <TargetFramework>net8.0</TargetFramework>
</PropertyGroup>

<!-- Multiple target frameworks -->
<PropertyGroup>
  <TargetFrameworks>net8.0;net7.0;net6.0</TargetFrameworks>
</PropertyGroup>
```

```bash
# Build for specific framework
dotnet build -f net8.0
dotnet build -f net7.0
```

### Setting Build Properties

```bash
# Set version
dotnet build -p:Version=1.2.3

# Set multiple properties
dotnet build -p:Configuration=Release -p:Platform=x64

# Enable specific features
dotnet publish -p:PublishSingleFile=true -p:PublishTrimmed=true
```

### Working with NuGet Configuration

```bash
# List NuGet sources
dotnet nuget list source

# Add a NuGet source
dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org

# Remove a source
dotnet nuget remove source nuget.org

# Enable a source
dotnet nuget enable source nuget.org

# Disable a source
dotnet nuget disable source nuget.org

# Update source credentials
dotnet nuget update source nuget.org --username user --password pass
```

---

## Advanced Scenarios

### Creating NuGet Packages

```bash
# Pack a project
dotnet pack

# Pack with specific version
dotnet pack -p:Version=1.0.0

# Pack in Release mode
dotnet pack -c Release

# Pack with symbols
dotnet pack --include-symbols --include-source

# Pack to specific output directory
dotnet pack -o ./nupkg

# Push to NuGet
dotnet nuget push MyPackage.1.0.0.nupkg --source https://api.nuget.org/v3/index.json --api-key <key>

# Push to local feed
dotnet nuget push MyPackage.1.0.0.nupkg --source ~/local-packages
```

### Working with Multiple Frameworks

```bash
# Build all target frameworks
dotnet build

# Build specific framework
dotnet build -f net8.0

# Run for specific framework
dotnet run -f net8.0

# Test specific framework
dotnet test -f net8.0
```

### Performance and Diagnostics

```bash
# Collect performance trace
dotnet trace collect --process-id <PID>

# Memory dump
dotnet dump collect --process-id <PID>

# Performance counters
dotnet counters monitor --process-id <PID>

# List running .NET processes
dotnet trace ps

# Analyze startup time
dotnet build -p:EventSourceSupport=true
```

### Code Formatting and Analysis

```bash
# Format code (requires dotnet-format tool)
dotnet format

# Verify formatting without changes
dotnet format --verify-no-changes

# Format specific folder
dotnet format ./src

# Analyze code
dotnet build -p:RunAnalyzers=true
dotnet build -p:EnforceCodeStyleInBuild=true
```

### Working with Docker

```dockerfile
# Example Dockerfile for .NET 8 app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyApp.csproj", "./"]
RUN dotnet restore
COPY . .
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
# Build with Docker
docker build -t myapp .

# Run with Docker
docker run -p 8080:8080 myapp
```

### Cleaning Build Artifacts

```bash
# Clean build output
dotnet clean

# Clean bin and obj folders recursively (PowerShell)
Get-ChildItem -Include bin,obj -Recurse | Remove-Item -Recurse -Force

# Clean bin and obj folders recursively (Bash)
find . -iname "bin" -o -iname "obj" | xargs rm -rf
```

---

## Tips and Best Practices

### General Tips

1. **Use .NET CLI for CI/CD**: All commands work across platforms
2. **Keep SDK Updated**: Regularly update with `dotnet --version`
3. **Use Solution Files**: Easier to manage multiple projects
4. **Leverage Watch Mode**: `dotnet watch run` for rapid development
5. **Global Tools**: Install frequently used tools globally
6. **Local Tools**: Use tool manifests for team consistency

### Project Organization

```
MyApplication/
├── src/
│   ├── MyApp.Api/
│   ├── MyApp.Core/
│   └── MyApp.Infrastructure/
├── tests/
│   ├── MyApp.UnitTests/
│   └── MyApp.IntegrationTests/
├── docs/
├── MyApplication.sln
└── README.md
```

### Configuration Best Practices

1. **Use User Secrets in Development**
   ```bash
   dotnet user-secrets set "ApiKey" "dev-key"
   ```

2. **Use Environment Variables in Production**
   ```bash
   export ConnectionStrings__DefaultConnection="Server=..."
   ```

3. **Use appsettings.{Environment}.json**
   - `appsettings.Development.json`
   - `appsettings.Production.json`

### Performance Tips

1. **Build in Release for Production**
   ```bash
   dotnet publish -c Release
   ```

2. **Use Ready2Run for Faster Startup**
   ```bash
   dotnet publish -c Release -p:PublishReadyToRun=true
   ```

3. **Enable Trimming for Smaller Deployments**
   ```bash
   dotnet publish -r linux-x64 -p:PublishTrimmed=true
   ```

4. **Use `--no-restore` and `--no-build` When Appropriate**
   ```bash
   dotnet build --no-restore
   dotnet test --no-build
   ```

### Common Shortcuts

```bash
# Create common project types quickly
alias dnc='dotnet new console -n'
alias dnw='dotnet new webapi -n'
alias dnm='dotnet new mvc -n'
alias dnx='dotnet new xunit -n'

# Common operations
alias dnb='dotnet build'
alias dnr='dotnet run'
alias dnt='dotnet test'
alias dnw='dotnet watch run'
```

### Troubleshooting

```bash
# Clear NuGet cache
dotnet nuget locals all --clear

# Verbose output for debugging
dotnet build -v detailed

# Check what files will be published
dotnet publish --no-build -v detailed

# Verify project references
dotnet list reference

# Check for missing dependencies
dotnet restore --verify-all-digests
```

---

## Quick Reference

### Project Creation
```bash
dotnet new console -n MyApp        # Console app
dotnet new webapi -n MyApi         # Web API
dotnet new mvc -n MyWeb            # MVC app
dotnet new classlib -n MyLib       # Class library
dotnet new xunit -n MyTests        # Test project
```

### Essential Commands
```bash
dotnet restore                     # Restore packages
dotnet build                       # Build project
dotnet run                         # Run project
dotnet test                        # Run tests
dotnet publish -c Release          # Publish for deployment
```

### Package Management
```bash
dotnet add package <name>          # Add package
dotnet remove package <name>       # Remove package
dotnet list package                # List packages
dotnet list package --outdated     # Check for updates
```

### Common Workflows
```bash
dotnet watch run                   # Auto-restart on changes
dotnet ef migrations add <name>    # Create migration
dotnet ef database update          # Apply migrations
dotnet user-secrets set <key> <val> # Set secret
```

---

## Additional Resources

- [Official .NET CLI Documentation](https://learn.microsoft.com/en-us/dotnet/core/tools/)
- [.NET SDK Downloads](https://dotnet.microsoft.com/download)
- [Runtime Identifier Catalog](https://learn.microsoft.com/en-us/dotnet/core/rid-catalog)
- [NuGet Package Manager](https://www.nuget.org/)
- [Entity Framework Core CLI](https://learn.microsoft.com/en-us/ef/core/cli/dotnet)
