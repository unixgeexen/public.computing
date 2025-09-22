.. raw:: html

    <button onclick="copyToClipboard()">Copy reStructuredText</button>
    <script>
        function copyToClipboard() {
            const el = document.createElement('textarea');
            el.value = document.documentElement.innerText;
            document.body.appendChild(el);
            el.select();
            document.execCommand('copy');
            document.body.removeChild(el);
            alert('Copied to clipboard!');
        }
    </script>

===========================================
AI-Enhanced Linux Development Environment
===========================================

Summary
-------
This guide provides step-by-step configurations for setting up an AI-powered development environment on Ubuntu, tailored for Unix/Linux system administration and personal productivity tools. It covers essential AI coding assistants (Qodo, GitHub Copilot), foundational Python libraries (TensorFlow, PyTorch), MLOps platforms (Charmed Kubeflow), and productivity enhancers (Jupyter, Kroki diagrams). All recommended tools are actively maintained with strong community support and include both open-source and commercial options. Procedures assume Ubuntu 22.04 LTS or newer with standard hardware requirements (minimum 8GB RAM, 20GB disk) :cite[5].

Recommended AI Tool Stack
-------------------------

.. csv-table:: AI Development Tools for Ubuntu
   :header: "Tool", "Category", "Key Features", "Setup Method"
   :widths: 20, 20, 35, 25

   "Qodo", "AI Coding Assistant", "Full SDLC coverage, multi-model support (OpenAI o1, Claude 3.5), automated testing", "VS Code/JetBrains marketplace"
   "GitHub Copilot", "AI Code Completion", "Real-time suggestions, multi-language support, chat functionality", "Native integration in supported IDEs"
   "TensorFlow", "Machine Learning", "Deep learning framework, production deployment ready", "pip install tensorflow"
   "PyTorch", "Machine Learning", "Research-friendly, dynamic computation graphs", "pip install torch torchvision"
   "Charmed Kubeflow", "MLOps Platform", "End-to-end ML workflows, Kubernetes-native", "snap install charmed-kubeflow"
   "Jupyter Notebook", "Productivity", "Interactive coding, visualization, documentation", "pip install notebook"
   "Kroki", "Diagramming", "Text-to-diagram conversion, multiple format support", "Docker container or native install"

System Preparation
------------------

Update Ubuntu and Install Core Dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash
   :copyicon:

   # Update package lists and upgrade existing packages
   sudo apt update && sudo apt upgrade -y
   
   # Install Python and essential build tools
   sudo apt install python3 python3-pip python3-venv build-essential -y
   
   # Install GPU support (optional for NVIDIA users)
   sudo apt install nvidia-driver-535 cuda-toolkit-12-0 -y

Python Environment Setup
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash
   :copyicon:

   # Create and activate virtual environment
   python3 -m venv ~/ai_venv
   source ~/ai_venv/bin/activate
   
   # Install core AI libraries
   pip install tensorflow torch torchvision scikit-learn pandas numpy

AI Tool Installation Procedures
-------------------------------

Qodo AI Assistant Setup
~~~~~~~~~~~~~~~~~~~~~~~

1. **Install in VS Code**:
   - Open VS Code, go to Extensions marketplace
   - Search for "Qodo AI" and install
   - Authenticate with Qodo account (free tier available)

2. **Command Line Installation**:
   .. code-block:: bash
      :copyicon:

      # Install via pip (alternative method)
      pip install qodo-ai
      qodo setup --ide=vscode

GitHub Copilot Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Subscription Required**:
   - Visit GitHub Copilot subscription page
   - Sign up for individual ($10/month) or team plan

2. **IDE Integration**:
   - Available for VS Code, JetBrains IDEs, Neovim
   - Authenticate via GitHub account in IDE settings :cite[4]

MLOps Platform Deployment
~~~~~~~~~~~~~~~~~~~~~~~~~

Charmed Kubeflow Installation for Production AI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
   :copyicon:

   # Install via snap
   sudo snap install charmed-kubeflow --classic
  
   # Initialize deployment
   charmed-kubeflow init --channel=latest/stable
  
   # Deploy core components
   charmed-kubeflow deploy :cite[1]

Productivity Tools Configuration
--------------------------------

Jupyter Notebook Setup
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash
   :copyicon:

   # Install Jupyter
   pip install notebook jupyterlab
   
   # Generate config
   jupyter notebook --generate-config
   
   # Set password
   jupyter notebook password
   
   # Start server (accessible at http://localhost:8888)
   jupyter notebook --ip=0.0.0.0 --port=8888

Kroki Diagramming Tool
~~~~~~~~~~~~~~~~~~~~~~

**Local Installation**:
.. code-block:: bash
   :copyicon:

   # Docker deployment (recommended)
   docker run -d -p 8000:8000 yuzutech/kroki

**Command Line Interface**:
.. code-block:: bash
   :copyicon:

   # Download Kroki CLI
   wget https://github.com/yuzutech/kroki-cli/releases/download/v0.1.0/kroki-cli_0.1.0_linux_amd64.tar.gz
   tar -xzf kroki-cli_0.1.0_linux_amd64.tar.gz
   sudo mv kroki /usr/local/bin/ :cite[10]

Example Usage: System Monitoring Dashboard
------------------------------------------

AI-Generated Python Script for System Monitoring
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python
   :copyicon:

   # Generated with Qodo AI - system monitoring dashboard
   import psutil
   import matplotlib.pyplot as plt
   from datetime import datetime
   
   def system_metrics():
       """Collect and display system metrics using AI-recommended parameters"""
       cpu_usage = psutil.cpu_percent(interval=1)
       memory = psutil.virtual_memory()
       disk = psutil.disk_usage('/')
       
       print(f"CPU Usage: {cpu_usage}%")
       print(f"Memory: {memory.percent}% used")
       print(f"Disk: {disk.percent}% full")
       
       # AI-suggested alert threshold
       if cpu_usage > 85:
           print("ALERT: High CPU usage detected")
   
   if __name__ == "__main__":
       system_metrics()

Kroki Diagram: AI Development Workflow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block::
   :copyicon:

   .. kroki::
      :type: plantuml
      
      @startuml
      title AI Development Workflow on Ubuntu
      start
      :Update System;
      :Install Python & Dependencies;
      :Setup Virtual Environment;
      :Install AI Tools;
      :Configure IDE Integration;
      :Test Installation;
      stop
      @enduml

Maintenance and Updates
-----------------------

Automated Update Script
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash
   :copyicon:

   #!/bin/bash
   # AI-enhanced system update script
   echo "Starting automated update process..."
   sudo apt update
   sudo apt upgrade -y
   pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U
   echo "Update complete! Run 'qodo check-updates' for AI tool updates"

Troubleshooting Common Issues
-----------------------------

.. csv-table:: Common Issues and Solutions
   :header: "Issue", "Solution", "Source"
   :widths: 30, 50, 20

   "GPU not detected", "Install NVIDIA drivers: sudo apt install nvidia-driver-535", ":cite[5]"
   "Python package conflicts", "Use virtual environments: python3 -m venv myenv", ":cite[5]"
   "Kroki service not starting", "Check ports: sudo lsof -i :8000", ":cite[10]"
   "Qodo authentication fails", "Renew API key: qodo auth --refresh", ":cite[4]"

References
----------
1. `Canonical MLOps <https://ubuntu.com/ai>`_
2. `Kroki Diagramming <https://kroki.io/>`_
3. `Qodo AI Assistant <https://www.qodo.ai/blog/best-ai-coding-assistant-tools/>`_
4. `GitHub Copilot <https://github.com/features/copilot>`_
5. `Ubuntu AI Setup <https://www.tecmint.com/setup-ai-development-environment-on-ubuntu/>`_

Metadata
--------
:Date: 2025-09-10
:Model: DeepSeek-R1
:Tags: #AIDevelopment #Ubuntu #SystemAdmin #MLOps #CodingAssistant

Context
-------
Original query: Configuration guide for AI code development tools on Ubuntu focused on system administration and productivity, with implementation procedures and well-supported tools.
