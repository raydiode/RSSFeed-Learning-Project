##### Module 0: Project Initialisation
**Overview**
This module covers setting up the foundational structure for our RSS aggregator project using Git for version control. We'll create a clean, well-organised project structure that will support our learning journey.

**Learning Objectives**
- Understanding basic Git concepts
- Creating and structuring a project directory
- Managing project documentation
- Handling sensitive data in version control

**Prerequisites**
- Git installed on your system
- GitHub account created
- Basic command line familiarity
- Text editor installed

**Step-by-Step Instructions**

1. **Create Project Directory**
```bash
# Create and navigate to project directory
mkdir rss-learning-project
cd rss-learning-project
```

2. **Initialise Git Repository**
```bash
# Initialise fresh Git repository
git init
```

3. **Create Project Structure**
```bash
# Create essential project files
touch .gitignore
touch README.md
mkdir docker-config
mkdir docs
```

4. **Configure .gitignore**
([[gitignore-explanation]])

```bash
# Add common ignore patterns to .gitignore
echo ".env
.env.*
docker-compose.override.yml
**/data/
**/log/
.DS_Store
node_modules/
*.log
" > .gitignore
```

5. **Create Initial README.md**
```bash
# Add basic project documentation
echo "# RSS Learning Project

A personal RSS aggregator built with FreshRSS, Docker, and WSL.

## Project Overview
This project is part of a guided learning journey using Claude.ai as a teaching assistant to build a privacy-focused news aggregator.

## Project Structure
- Environment setup
- FreshRSS deployment
- Feed configuration
- Security implementation
- Backup & version control
- Remote hosting

## Development Environment
- WSL
- Docker
- Git

## Getting Started
[Instructions will be added as project progresses]

## Notes
[Learning notes and observations will be added here]
" > README.md
```

6. **Initial Commit**
```bash
# Stage all files
git add .

# Create first commit
git commit -m "Initial project setup"
```

7. **Link to GitHub**
```bash
# Add remote repository (replace with your GitHub URL)
git remote add origin <your-github-repo-url>

# Push to main branch
git push -u origin main
```

**Success Criteria**
- Local Git repository initialised
- Project structure created
- Basic documentation in place
- .gitignore configured
- Repository linked to GitHub

**Common Issues and Solutions**
- If `git push` fails, ensure your GitHub repository exists and URL is correct
- If permission denied, check your GitHub authentication
- If files aren't being ignored, check .gitignore syntax

**Next Steps**
After completing this module, you should:
1. Review the project structure
2. Ensure all files are properly committed
3. Verify GitHub repository access
4. Prepare to move on to Module 1: Environment Setup

**Additional Resources**
- Git documentation: https://git-scm.com/doc
- GitHub guides: https://guides.github.com
- Markdown syntax: https://www.markdownguide.org/basic-syntax/

**Notes**
- Keep commit messages clear and descriptive
- Regularly check `git status` to monitor changes
- Consider using Git branches for different features as we progress
