# Complete Conda Environment Management Guide for Data Scientists

## Quick Reference Commands

### Essential Daily Commands
```bash
# Environment Navigation
conda env list                    # List all environments (* = active)
conda activate project_name       # Start working on project
conda deactivate                  # Return to base environment

# Package Management (2024 Best Practices)
mamba install pandas numpy        # Fastest option (install mamba first)
conda install pandas numpy        # Standard option
pip install package_name          # When package not in conda channels

# Environment Lifecycle
conda create -n project python=3.11     # Create new environment
conda env export > environment.yml      # Save complete setup
conda env create -f environment.yml     # Recreate environment anywhere
conda remove -n old_project --all       # Delete unused environment
```

### Quick Project Setup Templates
```bash
# Basic Data Science
conda create -n ds_project python=3.11 pandas numpy matplotlib seaborn scikit-learn jupyter

# Machine Learning
conda create -n ml_project python=3.11 pandas numpy scikit-learn xgboost jupyter
conda activate ml_project && pip install lightgbm optuna wandb

# Deep Learning
conda create -n dl_project python=3.11 pandas numpy matplotlib jupyter
conda activate dl_project && conda install tensorflow
```

---

## Complete Workflow: From Project Start to Finish

### Phase 1: Environment Setup

#### Step 1: Create Your Project Environment
```bash
# Modern approach: Python 3.11+ recommended for 2024
conda create -n my_project python=3.11

# Or create with common packages immediately
conda create -n my_project python=3.11 pandas numpy matplotlib jupyter

# Optional: Install mamba for faster package management
conda install mamba -c conda-forge
```

**Why Python 3.11+?** Better performance, latest library compatibility, and modern Python features.

#### Step 2: Activate and Verify Setup
```bash
conda activate my_project

# Verify environment
conda info                        # Shows active environment details
conda list                        # Shows installed packages
which python                      # Confirms Python path
```

### Phase 2: Package Management Strategy

#### Step 3: Install Packages in Correct Order (Critical for Success)
```bash
# FIRST: Install conda/mamba packages (better dependency resolution)
mamba install pandas numpy matplotlib seaborn scikit-learn jupyter
# Alternative: conda install pandas numpy matplotlib seaborn scikit-learn jupyter

# SECOND: Install specialized packages via pip (if not in conda-forge)
pip install lightgbm xgboost optuna wandb streamlit

# Verify installation
conda list                        # See all packages with sources
```

**Golden Rule:** Conda/mamba first, pip second. This prevents dependency conflicts and ensures proper environment isolation.

#### Step 4: Package Management Commands
```bash
# Search for packages
conda search package_name         # Check if available in conda
mamba search package_name         # Faster search with mamba

# Install specific versions
conda install pandas=1.5.0       # Pin to specific version
conda install -c conda-forge package_name  # From specific channel

# Update packages
mamba update pandas               # Update single package
conda update --all              # Update all packages (use sparingly)

# Remove packages
conda remove package_name        # Remove from current environment
```

### Phase 3: Environment Sharing & Backup

#### Step 5: Export Your Environment (The Right Way)
```bash
# RECOMMENDED: Complete environment export (handles conda + pip)
conda env export > environment.yml

# Alternative exports for specific use cases:
conda env export --no-builds > environment.yml     # More portable across platforms
conda env export --from-history > environment.yml  # Only explicitly installed packages
conda list --explicit > requirements.txt           # Exact reproduction (same OS only)
```

#### Step 6: Understanding Your Environment File
**Your `environment.yml` will look like this:**
```yaml
name: my_project
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11
  - pandas=1.5.0
  - numpy
  - scikit-learn
  - jupyter
  - pip
  - pip:
    - lightgbm
    - optuna
    - wandb
```

**Why YAML is Superior:**
- Automatically handles mixed conda/pip packages
- Installs conda packages first (better dependency resolution)
- Cross-platform compatible
- Industry standard for data science projects

### Phase 4: Environment Recreation

#### Step 7: Recreate Environment from YAML
```bash
# Create identical environment
conda env create -f environment.yml

# Create with different name
conda env create -f environment.yml -n different_name

# Update existing environment from YAML
conda env update -f environment.yml --prune
```

**Team Workflow:** Share `environment.yml` with teammates. They run one command and get identical setup.

### Phase 5: VS Code Integration

#### Step 8: VS Code Setup (2024 Method)
```bash
# After creating and activating environment
conda activate my_project

# Install Jupyter kernel for notebook support
conda install ipykernel
python -m ipykernel install --user --name my_project --display-name "Python (my_project)"
```

**VS Code Configuration:**
1. Open project folder in VS Code
2. `Ctrl+Shift+P` → "Python: Select Interpreter"
3. Select: `~/anaconda3/envs/my_project/bin/python`
4. VS Code now auto-activates this environment

**Verify Integration:**
- Terminal shows `(my_project)` prefix
- Jupyter notebooks show your environment in kernel list
- Python code uses packages from your environment

### Phase 6: Maintenance & Cleanup

#### Step 9: Environment Management
```bash
# List and navigate environments
conda env list                   # All environments (* = active)
conda info -n project_name      # Specific environment details
conda list -n project_name      # Packages in specific environment

# Switch between projects
conda activate project_a
conda deactivate
conda activate project_b
```

#### Step 10: System Cleanup
```bash
# Remove old environments
conda remove -n old_project --all

# Clean system (run monthly)
conda clean --all               # Remove cache and unused packages
conda clean --packages         # Just package cache
```

---

## Advanced Environment Management

### Environment Creation Options

#### Basic Creation Patterns
```bash
# Empty environment
conda create -n myenv python=3.11

# With essential packages
conda create -n myenv python=3.11 pandas numpy matplotlib

# Clone existing environment
conda create --clone existing_env --name new_env

# From requirements file
conda env create -f environment.yml
```

#### Custom Location and Channels
```bash
# Custom path (useful for shared drives)
conda create -p /path/to/myenv python=3.11

# Specific channels
conda create -n myenv -c conda-forge python=3.11 pandas

# Add default packages to all new environments
conda config --add create_default_packages pandas numpy matplotlib
```

### Channel Management
```bash
# Add conda-forge (recommended for more packages)
conda config --add channels conda-forge

# View channels
conda config --show channels

# Set strict channel priority (better performance)
conda config --set channel_priority strict

# Remove channel
conda config --remove channels channel_name
```

### Environment Variables
```bash
# Set environment-specific variables
conda env config vars set -n myenv API_KEY=your_key DATABASE_URL=localhost

# List variables
conda env config vars list -n myenv

# Remove variable
conda env config vars unset -n myenv API_KEY
```

---

## Data Science Environment Templates

### Template 1: Basic Data Analysis
```bash
conda create -n data_analysis python=3.11 \
    pandas numpy matplotlib seaborn \
    jupyter plotly statsmodels openpyxl
conda activate data_analysis
pip install ydata-profiling sweetviz
```

### Template 2: Machine Learning
```bash
conda create -n ml_project python=3.11 \
    pandas numpy matplotlib seaborn scikit-learn \
    jupyter xgboost
conda activate ml_project
pip install lightgbm optuna shap wandb mlflow
```

### Template 3: Deep Learning (CPU)
```bash
conda create -n dl_cpu python=3.11 \
    pandas numpy matplotlib jupyter
conda activate dl_cpu
conda install tensorflow scikit-learn
pip install tensorboard wandb
```

### Template 4: Deep Learning (GPU)
```bash
conda create -n dl_gpu python=3.11 \
    pandas numpy matplotlib jupyter
conda activate dl_gpu
conda install tensorflow-gpu  # or: conda install pytorch torchvision pytorch-cuda -c pytorch -c nvidia
pip install tensorboard wandb
```

### Template 5: Web Apps & Dashboards
```bash
conda create -n webapp python=3.11 \
    pandas numpy matplotlib
conda activate webapp
pip install streamlit plotly dash flask fastapi uvicorn
```

---

## Package Management Strategies

### Decision Matrix: Conda vs Pip vs Mamba

| Package Type | First Choice | Second Choice | Why |
|--------------|-------------|---------------|-----|
| Core data science (pandas, numpy) | `mamba install` | `conda install` | Better dependency resolution, faster |
| ML libraries (scikit-learn, xgboost) | `mamba install` | `conda install` | Optimized builds, GPU support |
| Specialized ML tools (optuna, wandb) | `pip install` | - | Latest versions, frequent updates |
| Web frameworks (streamlit, flask) | `pip install` | - | PyPI has latest versions |
| System libraries (opencv, gdal) | `conda install` | - | Complex system dependencies |

### Installation Best Practices
```bash
# 1. Create environment with core packages
conda create -n myproject python=3.11 pandas numpy matplotlib jupyter

# 2. Activate and install additional conda packages
conda activate myproject
mamba install scikit-learn seaborn plotly

# 3. Add pip packages last
pip install lightgbm optuna wandb streamlit

# 4. Verify no conflicts
conda list | grep pip        # See pip-installed packages
```

### Handling Package Conflicts
```bash
# If installation fails or conflicts occur:

# Option 1: Force reinstall
conda install --force-reinstall package_name

# Option 2: Try different channel
conda install -c conda-forge package_name

# Option 3: Use mamba for better solver
mamba install package_name

# Option 4: Create fresh environment
conda env export > backup.yml
conda remove -n myproject --all
conda env create -f backup.yml
```

---

## Environment Sharing Strategies

### Method Comparison

| Method | Use Case | Command | Pros | Cons |
|--------|----------|---------|------|------|
| **YAML Export** | Team sharing, mixed packages | `conda env export > environment.yml` | Handles conda+pip, cross-platform | Larger file size |
| **From History** | Clean sharing | `conda env export --from-history > environment.yml` | Only explicit packages | May miss dependencies |
| **Explicit List** | Exact reproduction | `conda list --explicit > requirements.txt` | Exact package versions | Platform-specific |
| **Pip Freeze** | Pure pip projects | `pip freeze > requirements.txt` | Simple, standard | Conda packages missing |

### Recommended Sharing Workflow
```bash
# For data science projects (RECOMMENDED):
conda env export > environment.yml

# Include in your project repository:
git add environment.yml
git commit -m "Add environment configuration"

# Team members recreate with:
conda env create -f environment.yml
```

### Creating Portable Environments
```bash
# More portable across different systems
conda env export --no-builds > environment.yml

# Minimal file with only explicit packages
conda env export --from-history > environment.yml

# For production deployment
conda list --explicit > requirements.txt
```

---

## Troubleshooting Guide

### Common Issues and Solutions

#### Issue 1: Environment Not Showing in VS Code
```bash
# Solution:
conda activate myenv
conda install ipykernel
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
# Restart VS Code and reselect interpreter
```

#### Issue 2: Slow Package Installation
```bash
# Solution: Install and use mamba
conda install mamba -c conda-forge
mamba install package_name  # Much faster than conda
```

#### Issue 3: Package Conflicts
```bash
# Solution 1: Use conda-forge channel
conda install -c conda-forge package_name

# Solution 2: Create fresh environment
conda env export > backup.yml
conda remove -n myenv --all
conda env create -f backup.yml
```

#### Issue 4: VS Code Terminal Not Activating Environment
```bash
# Solution:
conda init
# Restart terminal/VS Code
# If still issues, manually activate:
conda activate myenv
```

#### Issue 5: Mixed Conda/Pip Environment Issues
```bash
# Prevention: Always use this order
conda install conda_packages_first
pip install pip_packages_second

# Recovery: Recreate environment properly
conda env export > current_state.yml
# Edit YAML file to separate conda/pip packages properly
conda env create -f corrected_environment.yml
```

### Performance Optimization
```bash
# Install mamba for faster operations
conda install mamba -c conda-forge

# Use mamba as drop-in replacement for conda
mamba install package_name
mamba update --all
mamba search package_name

# Clean system regularly
conda clean --all  # Run monthly

# Set libmamba solver (conda 4.12+)
conda config --set solver libmamba
```

---

## Daily Workflow Examples

### Starting a New Project
```bash
# 1. Create and setup environment
conda create -n credit_model python=3.11 pandas numpy scikit-learn jupyter
conda activate credit_model

# 2. Install additional packages as needed
mamba install xgboost seaborn plotly
pip install optuna shap

# 3. Open VS Code with correct environment
code .
# Select Python interpreter: ~/anaconda3/envs/credit_model/bin/python

# 4. Save environment for team
conda env export > environment.yml
git add environment.yml
```

### Daily Development Routine
```bash
# Morning: Start work
conda activate my_current_project
code .  # VS Code opens with correct environment

# During development: Add packages as needed
mamba install new_package     # Try mamba first (fastest)
pip install specialized_tool  # Use pip for PyPI-only packages

# End of session: Update environment file
conda env export > environment.yml
```

### Weekly Maintenance
```bash
# Update packages (optional, be careful in active projects)
mamba update --all

# Clean up system
conda clean --all

# Review and remove unused environments
conda env list
conda remove -n unused_project --all
```

### Project Completion
```bash
# Final environment export
conda env export > final_environment.yml

# Archive environment
mv final_environment.yml project_archive/

# Remove environment to free space
conda remove -n completed_project --all
```

---

## Best Practices Summary

### Environment Management
✅ **DO:**
- One environment per project
- Use descriptive names (`fraud_detection`, not `env1`)
- Use Python 3.11+ for new projects
- Install mamba for faster package management
- Export environment.yml for each project
- Keep environment files in version control
- Clean up unused environments regularly

❌ **AVOID:**
- Installing everything in base environment
- Using old Python versions (3.8 or below) for new projects
- Random mixing of pip and conda without strategy
- Sharing requirements.txt for mixed environments
- Forgetting to activate environment before coding

### Package Installation Order
1. **Create environment** with Python version
2. **Install conda/mamba packages first** (better dependency resolution)
3. **Add pip packages last** (specialized tools)
4. **Export environment** for sharing/backup
5. **Test environment** on fresh installation

### Sharing and Collaboration
- Use `environment.yml` for mixed conda/pip projects
- Test environment recreation before sharing
- Document any special setup requirements
- Include environment files in project repositories
- Use descriptive environment names for team projects

### VS Code Integration
- Install ipykernel in each environment
- Select correct Python interpreter for each project
- Use integrated terminal for automatic environment activation
- Configure VS Code settings for seamless conda integration

---

## Command Reference Table

| Task | Command | Notes |
|------|---------|-------|
| **Environment Creation** |
| Create empty environment | `conda create -n myenv python=3.11` | Always specify Python version |
| Create with packages | `conda create -n myenv python=3.11 pandas numpy` | Faster than installing separately |
| Clone environment | `conda create --clone source_env --name new_env` | For testing variations |
| **Environment Management** |
| List environments | `conda env list` | * shows active environment |
| Activate environment | `conda activate myenv` | Start working in environment |
| Deactivate environment | `conda deactivate` | Return to base |
| Environment info | `conda info -n myenv` | Detailed environment information |
| **Package Management** |
| Install packages (fast) | `mamba install pandas numpy` | Requires mamba installation |
| Install packages (standard) | `conda install pandas numpy` | Standard conda method |
| Install from pip | `pip install package_name` | For PyPI-only packages |
| List packages | `conda list` | All packages in active environment |
| Update packages | `mamba update package_name` | Update specific package |
| Update all packages | `conda update --all` | Use sparingly in active projects |
| **Environment Sharing** |
| Export environment | `conda env export > environment.yml` | Complete environment with conda+pip |
| Export portable | `conda env export --no-builds > environment.yml` | More cross-platform compatible |
| Export minimal | `conda env export --from-history > environment.yml` | Only explicitly installed |
| Create from YAML | `conda env create -f environment.yml` | Recreate environment |
| Update from YAML | `conda env update -f environment.yml --prune` | Update existing environment |
| **Cleanup** |
| Remove environment | `conda remove -n myenv --all` | Delete entire environment |
| Clean system | `conda clean --all` | Remove cache and unused packages |
| Clean packages only | `conda clean --packages` | Just package cache |
| **Configuration** |
| Add channel | `conda config --add channels conda-forge` | Access more packages |
| Show configuration | `conda config --show` | View all settings |
| Set defaults | `conda config --set auto_activate_base false` | Customize behavior |

Remember: Always work in activated environments, use YAML files for sharing, and leverage mamba for faster operations. Keep environments clean, focused, and well-documented!