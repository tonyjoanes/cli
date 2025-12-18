# Command Line Tools Reference

> A comprehensive collection of command line tool documentation and guides for developers

## Overview

This repository serves as a centralized reference for commonly used command line tools and utilities. Whether you're working with Git, Docker, .NET, or system administration, you'll find practical, example-driven guides to help you be more productive.

## What's Inside

This repository contains detailed guides covering:

- **Version Control**: Git workflows and operations
- **Containerization**: Docker and container management
- **.NET Development**: .NET CLI tools and commands
- **Cloud Infrastructure**: Azure CLI for cloud resource management
- **System Administration**: Windows, Linux, and cross-platform utilities

Each guide is designed to be:
- ✅ **Practical** - Real-world examples you can use immediately
- ✅ **Comprehensive** - Covers common to advanced scenarios
- ✅ **Well-organized** - Easy to navigate and search
- ✅ **Example-driven** - Learn by seeing actual commands

## Documentation Guides

### Git
- **[Git Cheat Sheet](docs/git/cheatsheet.md)** - Complete guide to Git commands, workflows, and scenarios
  - Basic setup and configuration
  - Branching and merging strategies
  - Viewing changes with `git diff`
  - Creating and applying patches
  - Staging, committing, and undoing changes
  - Remote operations and collaboration
  - Stashing and history management
  - Advanced scenarios (cherry-pick, rebase, bisect)

### Docker
- **[Docker Commands Guide](docs/docker/commands-guide.md)** - Comprehensive Docker reference for containerization
  - Working with images and containers
  - Docker Compose for multi-container apps
  - Networking and volumes
  - Building optimized images
  - Debugging and monitoring
  - Common workflows (web apps, databases, CI/CD)
  - Best practices for security and performance

### .NET
- **[.NET CLI Guide](docs/dotnet/cli-guide.md)** - Essential .NET command line interface reference
  - Project and solution management
  - Building, running, and testing
  - NuGet package management
  - Publishing and deployment
  - Entity Framework Core operations
  - Tool management (global and local)
  - Common development workflows
  - Performance optimization tips

### Azure
- **[Azure CLI Guide](docs/azure/cli-guide.md)** - Complete Azure command line interface reference
  - Authentication and account management
  - Resource groups and resource management
  - Virtual machines and compute services
  - App Service and web applications
  - Storage accounts and data management
  - Databases (SQL, PostgreSQL, MySQL, Cosmos DB)
  - Networking (VNets, NSGs, load balancers)
  - Container services (ACI, ACR, AKS)
  - Azure Functions and serverless
  - Key Vault for secrets management
  - Monitoring and diagnostics

### Windows
- **[Windows Command Line Tips](docs/windows/tips-and-tricks.md)** - Windows CMD and PowerShell productivity guide
  - Command Prompt (CMD) essentials
  - PowerShell fundamentals
  - File and directory operations
  - System information and management
  - Networking commands
  - Process management
  - Productivity shortcuts and aliases

## Quick Start

### Browse Documentation
Simply navigate to the guide you need:

```bash
docs/
├── git/
│   └── cheatsheet.md
├── docker/
│   └── commands-guide.md
├── dotnet/
│   └── cli-guide.md
├── azure/
│   └── cli-guide.md
└── windows/
    └── tips-and-tricks.md
```

### Search for Commands
Use your browser's search (Ctrl+F / Cmd+F) within any guide to quickly find specific commands or scenarios.

### Bookmark for Quick Access
Keep this repository bookmarked for quick reference when you need to look up a command or workflow.

## How to Use These Guides

Each guide follows a consistent structure:

1. **Table of Contents** - Quick navigation to specific topics
2. **Getting Started** - Basic commands and setup
3. **Common Operations** - Day-to-day tasks and workflows
4. **Advanced Topics** - Complex scenarios and optimization
5. **Best Practices** - Tips, tricks, and recommendations
6. **Quick Reference** - Essential commands at a glance

### Example Usage

Looking to create a new .NET Web API project?
1. Open the [.NET CLI Guide](docs/dotnet/cli-guide.md)
2. Navigate to "Starting a New Web API Project"
3. Follow the step-by-step commands
4. Reference related sections as needed

Need to debug a Docker container?
1. Open the [Docker Commands Guide](docs/docker/commands-guide.md)
2. Check the "Debugging and Logs" section
3. Find the exact command you need
4. Apply it to your situation

## Who Is This For?

These guides are perfect for:

- 👨‍💻 **Developers** who need quick command reference
- 🎓 **Students** learning command line tools
- 🔧 **DevOps Engineers** managing infrastructure
- 📚 **Teams** standardizing on common workflows
- 🚀 **Anyone** who wants to be more productive with CLI tools

## Contributing

Found an error or want to add something? Contributions are welcome!

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

Please ensure your contributions:
- Follow the existing documentation style
- Include practical examples
- Are well-organized and clear
- Cover real-world use cases

## Philosophy

This documentation follows these principles:

- **Examples First** - Show, don't just tell
- **Real-World Focus** - Cover actual development scenarios
- **Progressive Complexity** - Start simple, build to advanced
- **Cross-Platform** - Works on Windows, macOS, and Linux where applicable
- **Always Current** - Keep documentation up-to-date

## Structure

```
cli/
├── docs/                   # All documentation
│   ├── git/               # Git guides
│   ├── docker/            # Docker guides
│   ├── dotnet/            # .NET CLI guides
│   ├── azure/             # Azure CLI guides
│   └── windows/           # Windows CLI guides
└── README.md              # This file
```

## Additional Resources

### Official Documentation
- [Git Documentation](https://git-scm.com/doc)
- [Docker Documentation](https://docs.docker.com/)
- [.NET CLI Documentation](https://learn.microsoft.com/en-us/dotnet/core/tools/)
- [Azure CLI Documentation](https://learn.microsoft.com/en-us/cli/azure/)
- [PowerShell Documentation](https://learn.microsoft.com/en-us/powershell/)

### Learning Resources
- [GitHub Git Handbook](https://guides.github.com/introduction/git-handbook/)
- [Docker Get Started](https://docs.docker.com/get-started/)
- [.NET Learn](https://dotnet.microsoft.com/learn)

## License

This documentation is provided as-is for educational and reference purposes.

## Feedback

Have suggestions or found something that could be improved?
- Open an issue
- Submit a pull request
- Share your feedback

---

**Happy coding! 🚀**

*Last updated: 2024*
