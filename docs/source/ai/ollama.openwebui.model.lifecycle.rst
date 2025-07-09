.. _model-lifecycle:

=============================================
Managing Model Lifecycle in Ollama/Open WebUI
=============================================

**Summary** (98 words)
This guide outlines lifecycle management for local AI models in an Ollama/Open WebUI environment on Linux. It covers model acquisition through Ollama's CLI and Open WebUI interface, configuration of quantization levels and system resources, storage management via cleanup commands, and status monitoring. The workflow ensures efficient utilization of local hardware while maintaining control over model versions and disk space. Both CLI and web-based approaches are documented, with Open WebUI automatically synchronizing with Ollama's local model registry.

***********************
Model Lifecycle Sections
***********************

Downloading Models
==================
1. **Via Ollama CLI**:

   .. code-block:: bash

      ollama pull <model-name>  # e.g. ollama pull llama3:8b

2. **Through Open WebUI**:
   - Navigate to http://localhost:3000
   - Select "Models" → "Download Model"
   - Search registry (e.g. "mistral") and select variant

Available Models
---------------
.. csv-table:: Common Model Tags
   :header: "Model", "Variants", "Size Range"
   :widths: 20, 30, 20

   "llama3", "8b, 70b, 8b-instruct", "4.7GB-40GB"
   "mistral", "7b, 7b-instruct", "4.1GB-6.8GB"
   "phi3", "mini, medium", "1.8GB-14GB"

Configuration
=============
1. **Custom Models**:

   .. code-block:: bash

      ollama create <custom-name> -f Modelfile
      # Example Modelfile:
      # FROM llama3:8b
      # PARAMETER num_ctx 4096

2. **System Limits**:
   Configure in ``~/.ollama/config.json``:

   .. code-block:: json

      {
        "num_gpu": 1,
        "num_thread": 8
      }

Model Status Checks
===================
.. csv-table:: Status Commands
   :header: "Command", "Description", "Example Output"
   :widths: 20, 40, 40

   "``ollama list``", "Lists downloaded models", "NAME            SIZE\nllama3:8b      4.7GB"
   "``ollama ps``", "Shows running instances", "ID      MODEL           CREATED"
   "Open WebUI → Models", "GUI model browser", "Visual status dashboard"

Cleaning Models
===============
1. **Remove Individual Model**:

   .. code-block:: bash

      ollama rm <model-name>

2. **Prune Unused Layers**:

   .. code-block:: bash

      ollama prune

3. **WebUI Cleanup**:
   - Models → Select model → Delete icon
   - Note: Only removes from UI visibility

**********
References
**********
- `Ollama Model Library <https://ollama.ai/library>`_
- `Open WebUI Model Docs <https://docs.openwebui.com/model-management/>`_
- `Ollama CLI Reference <https://github.com/ollama/ollama/blob/main/docs/cli.md>`_

**********
Metadata
**********
:Tags: ollama, open-webui, model-management, linux-server, local-ai, lifecycle
:Last Updated: 2024-06-13

**********
Context
**********
:Request Date: 2024-06-13
:Deepseek Parameters: temperature=0.3, top_p=0.95, max_tokens=1200
:Model: DeepSeek-R1-Lite-Preview
:Original Request: "Provide instructions to manage the lifecycle of models in a linux Ollama/Open WebUI environment. Include downloading models, configuring them for usage, cleaning up models, checking models available and the current model status"


