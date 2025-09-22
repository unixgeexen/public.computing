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

=====================
AI Tools Directory
=====================

Summary
-------
This directory lists 30 leading AI tools across 10 core categories, curated from comprehensive industry sources. Each entry includes the product name, category, brief description, and direct URL. Tools were selected based on functionality, market presence (2025), and specialized capabilities—prioritizing solutions with free tiers or trials where available :cite[1]:cite[10]. Notable inclusions range from established platforms like ChatGPT and Midjourney to emerging tools like Elicit for research automation and Qodo for agentic coding :cite[3]:cite[6].

AI Tools by Category
--------------------

.. csv-table:: AI Tools Directory (Category > Product > Description > URL)
   :header: "Category", "Product", "Description", "URL"
   :widths: 15, 20, 45, 20
   :align: left

   "Productivity", "ChatGPT", "Multimodal AI assistant for data analysis, summarization, and task automation", "https://chat.openai.com"
   "Productivity", "Notion AI", "AI-enhanced workspace for notes, tasks, and knowledge management", "https://www.notion.so/product/ai"
   "Productivity", "Fathom", "AI meeting assistant that records, transcribes, and summarizes calls", "https://fathom.video"
   "Coding", "GitHub Copilot", "AI pair programmer suggesting real-time code completions", "https://github.com/features/copilot"
   "Coding", "Qodo", "Agentic coding assistant for test generation, PR reviews, and code explanations", "https://qodo.ai"
   "Coding", "Tabnine", "AI-powered code completion with error detection and refactoring", "https://www.tabnine.com"
   "Image Generation", "Midjourney", "Generates artistic images from text prompts (Discord-based)", "https://www.midjourney.com"
   "Image Generation", "DALL·E 3", "OpenAI's image generator specializing in text-in-image accuracy", "https://openai.com/dall-e"
   "Image Generation", "GPT-4o (Image)", "Multimodal image generator within ChatGPT (text-to-image)", "https://chat.openai.com"
   "Video Creation", "Synthesia", "Creates AI avatar videos from text in 140+ languages", "https://www.synthesia.io"
   "Video Creation", "Runway ML", "AI video editor with generative tools and green screen removal", "https://runwayml.com"
   "Video Creation", "OpusClip", "Repurposes long videos into short clips optimized for social media", "https://opus.ai"
   "Research", "Perplexity", "AI search engine with source citations and Deep Research mode", "https://www.perplexity.ai"
   "Research", "Elicit", "Analyzes research papers (summarization, data extraction, synthesis)", "https://elicit.com"
   "Research", "Google AI Overviews", "Generates AI summaries of top search results", "https://labs.google.com/search"
   "Writing", "Jasper", "Enterprise content suite with 50+ templates for marketing copy", "https://www.jasper.ai"
   "Writing", "Copy.ai", "Generates marketing copy variations and blog outlines", "https://www.copy.ai"
   "Writing", "Gemini Advanced", "Google's writing assistant with 1M+ token context for long documents", "https://gemini.google.com"
   "Automation", "n8n", "Workflow automation platform connecting APIs and AI services", "https://n8n.io"
   "Automation", "Zapier", "Automates tasks between 5,000+ apps with AI agents", "https://zapier.com"
   "Automation", "Gumloop", "No-code workflow builder for custom AI automations", "https://gumloop.com"
   "Design", "Canva Magic Design", "AI layout generator for social media graphics and presentations", "https://www.canva.com/magic-design/"
   "Design", "Designs.AI", "All-in-one suite for logos, videos, and brand assets", "https://designs.ai"
   "Design", "Adobe Firefly", "Generative AI tool integrated into Photoshop/Express", "https://firefly.adobe.com"
   "Audio", "Descript", "Edits audio/video via text transcription and AI voice cloning", "https://www.descript.com"
   "Audio", "Turbo TTS", "High-quality text-to-speech with emotional resonance", "https://www.futurepedia.io/tool/turbo-tts"
   "Audio", "Fathom", "Records and summarizes meeting audio with speaker identification", "https://fathom.video"
   "Business", "C3 Generative AI", "Enterprise AI for contract analysis and financial benchmarking", "https://c3.ai/products/generative-ai/"
   "Business", "Anyword", "Generates data-driven marketing copy with performance predictions", "https://anyword.com"
   "Business", "Jasper", "Content creation platform for teams (marketing/sales focus)", "https://www.jasper.ai"

Category Insights
-----------------
- **Productivity**: AI tools dominate task automation (e.g., Fathom’s meeting summaries) and knowledge management :cite[2]:cite[10].
- **Coding**: Agentic assistants like Qodo automate testing and code reviews, reducing dev cycle times by 30-50% :cite[3].
- **Video/Audio**: Generative tools (Synthesia, Descript) cut production costs but require human oversight for brand alignment :cite[5]:cite[9].
- **Research**: Elicit processes 125M+ academic papers, enabling rapid systematic reviews :cite[6].

Metadata
--------
:Date: 2025-08-01
:Source: Industry reports, tool documentation, and user reviews
:Format: reStructuredText
:Tags: #AITools #Productivity #Coding #Design #TechDirectory

Context
-------
Original query: Comprehensive AI tools directory across 10 categories with product names, descriptions, and URLs. Replaces earlier Futurepedia table.
