##### RSS Aggregator Learning Project - A Guided Learning Journey

This document outlines a self-guided learning project to build a personalised news and entertainment aggregator using FreshRSS. The project focuses on privacy, clean reading experience, and accessibility. Claude.ai is being used as an AI teaching assistant throughout this journey - the provided prompt template helps maintain context and learning continuity across sessions. **Claud.ai was used to produce a summary of instructions for each module based on the conversations that took place during implementation**.

**Project Aims**
This project combines practical skills in modern development tools and practices whilst building a useful personal application. We'll use the command line interface (CLI) wherever possible to build strong foundational skills in development.

**Learning Philosophy**
Each module builds upon previous knowledge, emphasising understanding over simple completion. The project takes a practical, hands-on approach where we learn by doing, with Claude.ai providing guidance and explanations at each step.

##### Core Technical Skills Covered
- Command-line interface proficiency and WSL (Windows Subsystem for Linux)
- Docker container management and deployment
- Git version control and project management
- Web application configuration and customisation
- Security implementation and best practices
- Cloud hosting and remote access setup

##### Project Modules

**Module 0: Project Initialisation**
Understanding version control is crucial for modern development. We begin by setting up our project structure and Git repository before any other work begins. This ensures we can track changes, document our learning, and maintain a clean project history.
- Create project directory structure
- Initialise Git repository
- Set up .gitignore for sensitive files
- Create documentation foundation
- Link with GitHub remote repository

**Module 1: Environment Setup** 
A solid development environment is fundamental to any project. We'll take a systematic approach to verify our existing tools before making any changes, ensuring we have exactly what we need for development and testing.

- Verify WSL status and configuration (version, distribution, status)
- Check Docker Desktop installation and WSL 2 backend integration
- Test Docker functionality with a simple container
- Document our working environment configuration
- Install only missing components if needed
- Understand basic system verification commands

**Module 2: FreshRSS Deployment**
Docker containers provide consistency and portability. We'll learn container basics while setting up our core application.
- Create and configure FreshRSS container
- Understand Docker networking and ports
- Access and test web interface
- Learn container management basics

**Module 3: Feed Configuration**
Content aggregation requires understanding of RSS feeds. We'll set up various news sources and ensure quality content delivery.
- Configure multiple news sources
- Implement full-text extraction
- Configure content cleaning
- Optimise reading experience

**Module 4: Remote Hosting**
Making our application accessible requires understanding of cloud hosting and networking. We'll deploy our project to Cloud for anywhere-access.
- Configure Cloud instance
- Enable secure remote access
- Configure mobile accessibility
- Other security considerations

##### Context Prompt for Claude.ai
When starting work on any module, use this prompt template to maintain context:
```
I'm working on Module [X] of my RSS Aggregator project. My current objective is [specific objective]. My development environment is WSL with Docker. My experience level is beginner, and I'm looking to understand each step thoroughly. This project is being guided by Claude.ai. Please help me with [specific task/question].
```

**Required Tools**
- Windows PC with WSL enabled
- Docker Desktop for Windows
- Git and GitHub account
- Text editor 
- Modern web browser
- Terminal application

**Project Success Notes**
1. Take time to understand concepts before moving forward
2. Document challenges and solutions in your README
3. Commit changes regularly with meaningful messages
4. Test thoroughly after each significant change
5. Keep security in mind throughout the project
6. Ask questions when concepts aren't clear

Each module includes practical exercises and real-world applications. Reference materials and specific guidance will be provided during each module as needed.

##### Getting Started
Begin by cloning this repository and reviewing the Project Structure section of the README.md file. Ensure all required tools are installed before proceeding with Module 0.
