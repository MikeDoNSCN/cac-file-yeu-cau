You are an expert software engineer. Build a complete AI Gateway MCP Server based on this PRD below.

PATH VARIABLES (DEFINE THESE BEFORE STARTING):
- %MYRAY_FACTORY% = Your MyRay Factory root directory
- %AI_GATEWAY_MCP% = %MYRAY_FACTORY%\ai-gateway-mcp
- %LOCAL_MODELS% = Your local GGUF models directory

PROJECT LOCATION:
Create all files in: %AI_GATEWAY_MCP%

OUTPUT STRUCTURE:
%AI_GATEWAY_MCP%\
├── src\           # All Python backend code
├── web-ui\        # React TypeScript frontend
├── build.py       # Build script for Windows executable
├── requirements.txt
├── package.json
└── README.md

CRITICAL REQUIREMENTS:
1. **NO HORIZONTAL DEPENDENCIES** - Only vertical (downward) dependencies allowed
2. **PRODUCTION READY** - Every feature must work from Day 1, no placeholders
3. **Each system starts with LOCAL MODELS by default**, can switch to cloud later
4. **Manual mode is DEFAULT for AI Gateway** (user controls model selection)
5. **Beautiful React GUIs** with animations and real-time updates
6. **Comprehensive logging** with GUI viewers in each system
7. **Error handling** with graceful recovery
8. **Zero configuration** - works immediately after build

DEFAULT CONFIGURATION:
```json
{
  "DEFAULT_MODE": "manual",  // CRITICAL: System MUST start in manual mode
  "DEFAULT_LOCAL_MODEL": "llama-3.2-3b-instruct.Q4_K_M.gguf",
  "LOCAL_MODELS_PATH": "%LOCAL_MODELS%",
  "MCP_PORT": 3000,
  "WEB_PORT": 8080
}
```

PATH VARIABLES (DEFINE THESE BEFORE STARTING):
%MYRAY_FACTORY% = Your MyRay Factory root directory
%AI_GATEWAY_MCP% = %MYRAY_FACTORY%\ai-gateway-mcp
%LEXICON_MCP_SERVER% = %MYRAY_FACTORY%\lexicon-mcp-server  
%LOCAL_CONTEXT_MANAGER% = %MYRAY_FACTORY%\local-context-manager
%LOCAL_MODELS% = Your local GGUF models directory

PATH RULES:
1. Define %MYRAY_FACTORY% first (e.g., %MYRAY_FACTORY%)
2. All other paths derive from %MYRAY_FACTORY%
3. NEVER use relative paths (../, ./, etc.)
4. Use backslashes (\) for Windows paths
5. Create parent directories if they don't exist
6. Always use variables, never hardcode paths

PROJECT LOCATION:
Create all files in: %AI_GATEWAY_MCP%

OUTPUT STRUCTURE:
%AI_GATEWAY_MCP%\
├── src\           # All Python backend code
├── web-ui\        # React TypeScript frontend
├── build.py       # Build script for Windows executable
├── requirements.txt
├── package.json
└── README.md

Generate COMPLETE working code with:
- Full error handling and recovery
- Comprehensive test suites
- All features from Day 1
- No external dependencies except those in requirements.txt
- NO PLACEHOLDERS OR TODOs

IMPORTANT: Read the entire PRD below before starting implementation.
---
PRD:

# AI Gateway MCP Server - Product Requirements Document
## Centralized AI Model API Management with Manual Control by Default

## Overview

AI Gateway is a production-ready MCP server that provides a unified interface for managing and accessing multiple AI providers including cloud services (OpenAI, Anthropic, Google, OpenRouter, Kimi) and local GGUF models. **THE SYSTEM STARTS IN MANUAL MODE BY DEFAULT**, giving users complete control over model selection. ModelBot agent provides suggestions but NEVER overrides user choices in manual mode.

### Core Principles

1. **USER CONTROL FIRST**: System defaults to MANUAL MODE where users explicitly choose models
2. **LOCAL MODELS DEFAULT**: On startup, detect and load GGUF models from %LOCAL_MODELS%
3. **ZERO HORIZONTAL DEPENDENCIES**: Gateway is a standalone MCP server on port 3000
4. **MODELBOT RESPECTS BOUNDARIES**: In manual mode, ModelBot only suggests, never acts
5. **PRODUCTION READY**: Ships as a portable Windows EXE with zero configuration needed

---

## Architecture

```
┌─────────────────┐     MCP Protocol      ┌──────────────────┐
│ Context Manager │◄─────────────────────►│ AI Gateway MCP   │
│ (MCP Client)    │      Port 3000        │     Server       │
└─────────────────┘                       └────────┬─────────┘
                                                   │
                                          ┌────────▼─────────┐
                                          │   Web GUI        │
                                          │  (Port 8080)     │
                                          └────────┬─────────┘
                                                   │
          ┌────────────────────┬─────────────────┬─────────────────┐
          │                    │                 │                 │
    ┌─────▼──────┐      ┌─────▼──────┐   ┌─────▼──────┐   ┌──────▼─────┐
    │   Cloud    │      │   Local     │   │   KIMI     │   │   GGUF     │
    │ Providers  │      │   Servers   │   │  Models    │   │   Models   │
    ├────────────┤      ├─────────────┤   ├────────────┤   ├────────────┤
    │ • OpenAI   │      │ • Ollama    │   │ • Kimi K2  │   │ • Detected │
    │ • Anthropic│      │ • LM Studio │   │ • Kimi Dev │   │   from     │
    │ • Google   │      │ • LocalAI   │   │            │   │   %LOCAL_  │
    │ • OpenRouter      └─────────────┘   └────────────┘   │   MODELS%  │
    │ • Groq     │                                          └────────────┘
    └─────────────┘
```

---

## Operating Modes

### 1. MANUAL MODE (DEFAULT) ⭐
- **System starts in this mode ALWAYS**
- User has complete control over model selection
- ModelBot provides information only, no automatic actions
- Every model switch requires explicit user action
- Configuration: `"DEFAULT_MODE": "manual"`

### 2. SUGGEST MODE
- ModelBot actively suggests better models
- Shows cost savings and performance improvements
- User must approve each suggestion
- One-click to accept or dismiss suggestions

### 3. AUTO MODE
- ModelBot can automatically switch models
- User sets rules and constraints
- All actions are logged and reversible
- Requires explicit opt-in

### 4. HYBRID MODE
- Combines manual control with selective automation
- User defines which tasks can be automated
- Maintains manual control for sensitive operations

---

## Core Features

### 1. Local Model Startup (CRITICAL)

```python
# On startup, MUST detect local models
LOCAL_MODELS_PATH = os.environ.get('LOCAL_MODELS', os.path.expandvars('%LOCAL_MODELS%'))

def detect_gguf_models():
    """Detect all GGUF models in the specified directory"""
    models = []
    for file in Path(LOCAL_MODELS_PATH).glob("*.gguf"):
        models.append({
            "name": file.stem,
            "path": str(file),
            "size": file.stat().st_size,
            "type": "gguf"
        })
    
    # Default model if available
    default = "llama-3.2-3b-instruct.Q4_K_M.gguf"
    if any(m['name'] == default.replace('.gguf', '') for m in models):
        load_model(default)
    
    return models
```

Expected models to detect:
- llama-3.2-3b-instruct.Q4_K_M.gguf (default)
- qwen2.5-coder-7b-instruct.Q4_K_M.gguf
- gemma-2-2b-it-abliterated.Q6_K_L.gguf
- Llama-3.2-1B-Instruct-Q8_0.gguf
- And any other GGUF files in the directory
### 2. ModelBot Agent Implementation

```python
class ModelBot:
    """AI operations assistant that respects user control"""
    
    def __init__(self, mode="manual"):
        self.mode = mode  # ALWAYS starts as "manual"
        self.learning_enabled = True
        self.pattern_memory = {}
        
    async def analyze_request(self, prompt, context):
        """Analyze but don't act in manual mode"""
        analysis = {
            "recommended_model": self.find_best_model(prompt),
            "reasoning": self.explain_recommendation(),
            "cost_savings": self.calculate_savings(),
            "quality_impact": self.assess_quality()
        }
        
        if self.mode == "manual":
            # In manual mode, ONLY return information
            return {"analysis": analysis, "action": None}
        elif self.mode == "suggest":
            return {"analysis": analysis, "suggestion": self.create_suggestion()}
        elif self.mode == "auto":
            return {"analysis": analysis, "action": self.create_action()}
    
    def respect_user_control(self):
        """Core principle: User is always in control"""
        return self.mode == "manual"
```

### 3. API Key Management
- **Secure Storage**: Encrypt and store API keys locally using Windows DPAPI
- **Kimi Integration**: Special handling for Kimi API keys and endpoints
- **One-time Setup**: Enter keys once, validate automatically
- **Visual Indicators**: Show which providers are configured
- **Test Connection**: Validate before saving

### 4. Kimi Model Support (NEW)

```python
# Kimi provider configuration
KIMI_MODELS = {
    "kimi-k2": {
        "name": "Kimi K2",
        "description": "Fast reasoning model with 128K context",
        "context_length": 128000,
        "features": ["reasoning", "fast", "cost-effective"],
        "endpoints": {
            "direct": "https://api.kimi.ai/v1/chat/completions",
            "openrouter": "kimi/k2-latest",
            "groq": "kimi-k2-128k"
        }
    },
    "kimi-dev": {
        "name": "Kimi Dev",
        "description": "Coding optimized model with 256K context",
        "context_length": 256000,
        "features": ["coding", "debugging", "large-context"],
        "endpoints": {
            "direct": "https://api.kimi.ai/v1/chat/completions",
            "openrouter": "kimi/dev-latest"
        }
    }
}
```

### 5. Favorites System with Keyboard Shortcuts
- **Complete Configuration**: Provider, model, API key ref, all settings
- **Keyboard Shortcuts**: Press 1-9 to activate first 9 favorites
- **Visual Cards**: Rich display with all information
- **One-Click Switch**: Instant model switching
- **MCP Activation**: Can be triggered by Context Manager

```javascript
// Keyboard shortcut implementation
useEffect(() => {
    const handleKeyPress = (e) => {
        if (e.key >= '1' && e.key <= '9') {
            const index = parseInt(e.key) - 1;
            if (favorites[index]) {
                activateFavorite(favorites[index].id);
            }
        }
    };
    window.addEventListener('keypress', handleKeyPress);
    return () => window.removeEventListener('keypress', handleKeyPress);
}, [favorites]);
```

### 6. Zero Horizontal Dependencies
- **Standalone Server**: Gateway knows nothing about Lexicon or Context Manager
- **MCP Server Interface**: Provides tools on port 3000
- **Client-Server Model**: Other systems connect to Gateway
- **No Imports**: No dependencies on other MyRay systems
- **Self-Contained**: All functionality within Gateway boundaries

---

## MCP Tools Specification

### Core AI Tools

```python
@mcp_tool(name="ai.complete")
async def complete(
    prompt: str,
    model: str = None,
    provider: str = None,
    temperature: float = 0.7,
    max_tokens: int = None,
    system: str = None,
    favorite_id: str = None,
    metadata: dict = None  # For tracking by Context Manager
) -> dict:
    """Send completion request to AI model"""
    return {
        "response": str,
        "model": str,
        "provider": str,
        "mode": str,  # current mode (manual/suggest/auto)
        "usage": {
            "prompt_tokens": int,
            "completion_tokens": int,
            "total_cost": float
        },
        "latency_ms": int,
        "metadata": dict  # Echo back for tracking
    }

@mcp_tool(name="ai.stream")
async def stream_complete(
    prompt: str,
    model: str = None,
    provider: str = None,
    temperature: float = 0.7,
    max_tokens: int = None,
    favorite_id: str = None
) -> dict:
    """Stream completion response"""
    # Returns streaming response with chunks

# Coordination Tools for Context Manager

@mcp_tool(name="gateway.get_status")
async def get_gateway_status() -> dict:
    """Get current gateway status for coordination"""
    return {
        "status": "active",
        "mode": str,  # manual/suggest/auto/hybrid
        "active_model": {
            "provider": str,
            "model": str,
            "context_length": int
        },
        "available_providers": list,
        "local_models_loaded": list,
        "modelbot_status": str,
        "uptime_seconds": int,
        "total_requests": int
    }

@mcp_tool(name="modelbot.ask")
async def ask_modelbot(
    question: str,
    context: dict = None
) -> dict:
    """Query ModelBot for recommendations"""
    return {
        "recommendation": str,
        "reasoning": str,
        "alternatives": list,
        "confidence": float,
        "mode": str  # Current operating mode
    }

@mcp_tool(name="favorites.activate_by_number")
async def activate_favorite_by_number(
    number: int  # 1-9 for keyboard shortcuts
) -> dict:
    """Activate favorite by keyboard shortcut number"""
    return {
        "status": "activated",
        "favorite": dict,
        "previous_model": dict
    }

# Provider Management

@mcp_tool(name="ai.list_models")
async def list_models(
    provider: str = None,
    include_local: bool = True,
    include_kimi: bool = True,
    refresh: bool = False
) -> dict:
    """List available models including Kimi"""
    return {
        "models": [
            {
                "id": str,
                "name": str,
                "provider": str,
                "type": str,  # "cloud", "local", "gguf", "kimi"
                "context_length": int,
                "capabilities": list,
                "status": str  # "ready", "loading", "unavailable"
            }
        ],
        "local_models": list,  # GGUF models detected
        "kimi_models": list,   # Kimi K2 and Dev
        "timestamp": str
    }

# Logging and Monitoring

@mcp_tool(name="logs.query")
async def query_logs(
    start_date: str = None,
    end_date: str = None,
    level: str = None,
    provider: str = None,
    search: str = None,
    limit: int = 100
) -> dict:
    """Query application logs with GUI viewer support"""
    return {
        "logs": list,
        "total_count": int,
        "query_time_ms": int
    }
```

---

## Web GUI Detailed Design

### Main Interface Layout (Dark Theme)

```
┌─────────────────────────────────────────────────────────────────┐
│  AI Gateway  [📊 Stats] [📋 Logs] [⚙️ Settings] [❓ Help]       │
│  Mode: [🔒 MANUAL ▼] ← System is in MANUAL mode (default)       │
├─────────────────────────────────────────────────────────────────┤
│ ┌─────────────────┐ ┌──────────────────────────────────────────┐│
│ │ 🔐 Providers    │ │ ⭐ Favorites (Press 1-9 for quick access)││
│ ├─────────────────┤ ├──────────────────────────────────────────┤│
│ │ LOCAL GGUF ✓ 📁│ │ ┌────────────┐ ┌────────────┐ ┌────────┐││
│ │ 5 models loaded │ │ │[1] Local   │ │[2] Code    │ │[3] Chat│││
│ ├─────────────────┤ │ │   Llama    │ │   Helper   │ │   Bot  │││
│ │ OpenAI     ✓ 🔑│ │ │ GGUF 3.2B  │ │ GPT-4      │ │ Kimi K2│││
│ │ Anthropic  ✓ 🔑│ │ │ Fast local │ │ 128K ctx   │ │ 128K   │││
│ │ Google     ✗ 🔑│ │ │ No cost    │ │ $0.03/1K   │ │ Fast   │││
│ │ Kimi       ✓ 🔑│ │ └────────────┘ └────────────┘ └────────┘││
│ │ OpenRouter ✓ 🔑│ │ [+ Add Favorite]                         ││
│ └─────────────────┘ └──────────────────────────────────────────┘│
│                                                                  │
│ ┌─────────────────┐ ┌──────────────────────────────────────────┐│
│ │ 🤖 ModelBot     │ │ 📝 Test Chat                             ││
│ ├─────────────────┤ ├──────────────────────────────────────────┤│
│ │ Status: Ready   │ │ Active: llama-3.2-3b (LOCAL GGUF)       ││
│ │ Mode: Manual    │ │ Message: [........................] [Send]││
│ │                 │ │                                           ││
│ │ 💡 Suggestion:  │ │ Response:                                ││
│ │ "For this task,│ │ ┌────────────────────────────────────────┐││
│ │ GPT-3.5 would  │ │ │                                        │││
│ │ save $0.02"    │ │ │ AI response appears here...            │││
│ │                 │ │ │                                        │││
│ │ [View Details] │ │ └────────────────────────────────────────┘││
│ └─────────────────┘ │ Tokens: 150 | Cost: $0.00 | Time: 0.3s  ││
│                     └──────────────────────────────────────────┘│
│ ┌─────────────────────────────────────────────────────────────┐│
│ │ 📊 Live Stats                                   [Detailed →] ││
│ ├─────────────────────────────────────────────────────────────┤│
│ │ Requests Today: 142 | Tokens: 45.2K | Cost: $12.30         ││
│ │ Local Model Usage: 68% | Cloud Usage: 32% | Errors: 0      ││
│ │ Current Load: ▂▄▆█▆▄▂ | Response Time: ▁▂▁▃▁▂▁           ││
│ └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

### Local Model Setup Wizard (First Run)

```
┌─────────────────────────────────────────────────────────────────┐
│ 🚀 Welcome to AI Gateway - Local Model Setup                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ 📁 Detecting models in: %LOCAL_MODELS%\                          │
│                                                                  │
│ Found 5 GGUF models:                                            │
│ ┌──────────────────────────────────────────────────────────┐   │
│ │ ✓ llama-3.2-3b-instruct.Q4_K_M.gguf (2.4 GB)    [Load] │   │
│ │ ○ qwen2.5-coder-7b-instruct.Q4_K_M.gguf (4.9 GB) [Load] │   │
│ │ ○ gemma-2-2b-it-abliterated.Q6_K_L.gguf (2.1 GB) [Load] │   │
│ │ ○ Llama-3.2-1B-Instruct-Q8_0.gguf (1.3 GB)      [Load] │   │
│ │ ○ Phi-3.5-mini-instruct-Q4_K_M.gguf (2.5 GB)     [Load] │   │
│ └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│ 💡 Recommended: Load at least one model to start                │
│                                                                  │
│ [Load Default] [Select Models] [Add Directory] [Skip]           │
└─────────────────────────────────────────────────────────────────┘
```

### ModelBot Configuration Panel

```
┌─────────────────────────────────────────────────────────────────┐
│ 🤖 ModelBot Configuration                              [✖]      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│ Operating Mode:                                                  │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ 🔒 Manual (Default)                                     [✓] │ │
│ │    You have complete control, ModelBot only informs         │ │
│ │                                                              │ │
│ │ 💡 Suggest                                              [ ] │ │
│ │    ModelBot suggests optimizations you can accept/reject    │ │
│ │                                                              │ │
│ │ 🔄 Auto                                                 [ ] │ │
│ │    ModelBot can switch models based on your rules          │ │
│ │                                                              │ │
│ │ 🎯 Hybrid                                               [ ] │ │
│ │    Selective automation for specific tasks                  │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│ Learning & Optimization:                                         │
│ [✓] Learn from usage patterns                                   │
│ [✓] Track cost savings                                          │
│ [✓] Monitor response quality                                    │
│ [ ] Share anonymous insights (helps improve ModelBot)           │
│                                                                  │
│ Automation Rules (when not in Manual mode):                     │
│ • Max cost difference for auto-switch: [50%▼]                   │
│ • Minimum quality threshold: [80%▼]                             │
│ • Require approval for prompts > [100▼] tokens                  │
│                                                                  │
│ [Save Configuration] [Reset to Defaults] [Cancel]               │
└─────────────────────────────────────────────────────────────────┘
```

---

## Implementation Architecture

### Directory Structure

```
ai-gateway-mcp/
├── src/
│   ├── main.py              # Entry point - starts both servers
│   ├── config.py            # DEFAULT_MODE = "manual" configuration
│   ├── mcp/
│   │   ├── server.py        # MCP server on port 3000
│   │   ├── tools.py         # All MCP tool implementations
│   │   └── protocol.py      # MCP protocol handling
│   ├── providers/
│   │   ├── base.py          # Abstract base provider
│   │   ├── openai.py        # OpenAI implementation
│   │   ├── anthropic.py     # Anthropic implementation
│   │   ├── google.py        # Google implementation
│   │   ├── kimi.py          # Kimi K2 and Dev implementation
│   │   ├── openrouter.py    # OpenRouter implementation
│   │   ├── gguf.py          # Local GGUF model handler
│   │   └── registry.py      # Provider registration
│   ├── modelbot/
│   │   ├── agent.py         # ModelBot core agent
│   │   ├── analyzer.py      # Request analysis
│   │   ├── learner.py       # Pattern learning
│   │   └── rules.py         # Automation rules
│   ├── storage/
│   │   ├── keys.py          # Encrypted key storage (Windows DPAPI)
│   │   ├── favorites.py     # Favorites with keyboard shortcuts
│   │   ├── config.py        # User preferences
│   │   └── database.py      # SQLite for logs/stats
│   ├── web/
│   │   ├── server.py        # FastAPI web server on port 8080
│   │   ├── routes.py        # API endpoints
│   │   └── static/          # React build output
│   └── utils/
│       ├── gguf_detector.py # GGUF model detection
│       ├── encryption.py    # Windows DPAPI wrapper
│       └── validators.py    # Input validation
├── web-ui/                  # React TypeScript source
│   ├── src/
│   │   ├── components/
│   │   │   ├── Dashboard.tsx
│   │   │   ├── ModelBot.tsx
│   │   │   ├── Favorites.tsx
│   │   │   └── LocalModels.tsx
│   │   ├── hooks/
│   │   │   ├── useKeyboardShortcuts.ts
│   │   │   └── useModelBot.ts
│   │   └── App.tsx
│   └── package.json
├── build.py                # Windows EXE build script
├── requirements.txt        # Python dependencies
└── README.md              # Quick start guide
```

---

## Security Considerations

1. **Windows DPAPI Integration**:
```python
# src/utils/encryption.py
import win32crypt
import json

class WindowsKeyStorage:
    """Secure key storage using Windows DPAPI"""
    
    def encrypt_key(self, provider: str, api_key: str):
        """Encrypt API key using DPAPI"""
        data = json.dumps({
            "provider": provider,
            "key": api_key,
            "timestamp": datetime.now().isoformat()
        }).encode()
        
        encrypted = win32crypt.CryptProtectData(
            data,
            f"AI Gateway - {provider}",
            None, None, None, 0
        )
        return encrypted
    
    def decrypt_key(self, encrypted_data: bytes):
        """Decrypt API key using DPAPI"""
        decrypted = win32crypt.CryptUnprotectData(
            encrypted_data,
            None, None, None, 0
        )
        return json.loads(decrypted[1].decode())
```

2. **Local Model Security**:
   - Models stay on local machine
   - No external API calls for GGUF models
   - Process isolation for model inference
   - Memory protection for loaded models

3. **MCP Security**:
   - Server binds to localhost only
   - No external network access
   - Request validation on all inputs
   - Rate limiting per client

---

## Performance Requirements

1. **Startup Performance**:
   - Local model detection: < 1s
   - Default model loading: < 5s
   - GUI ready: < 2s
   - MCP server ready: < 1s

2. **Runtime Performance**:
   - Model switching: < 100ms
   - Favorite activation: < 50ms
   - Keyboard shortcuts: < 16ms response
   - Local inference: Depends on model size

3. **Resource Usage**:
   - Base memory: < 200MB
   - With GGUF model: + model size
   - CPU idle: < 5%
   - Disk space: < 100MB (excluding models)

---

## Build Configuration

### Windows Executable Build

```python
# build.py
import PyInstaller.__main__
import shutil
import os

def build_windows_exe():
    """Build standalone Windows executable"""
    
    # Build React UI first
    print("Building React UI...")
    os.system("cd web-ui && npm run build")
    
    # Copy to static
    shutil.copytree("web-ui/build", "src/web/static", dirs_exist_ok=True)
    
    # PyInstaller configuration
    PyInstaller.__main__.run([
        'src/main.py',
        '--onefile',
        '--windowed',
        '--name=AI-Gateway',
        '--icon=assets/icon.ico',
        '--add-data=src/web/static;web/static',
        '--add-data=src/config.py;.',
        '--hidden-import=win32crypt',
        '--hidden-import=gguf',
        '--collect-all=openai',
        '--collect-all=anthropic',
        '--collect-all=google.generativeai',
        '--collect-all=fastapi',
        '--collect-all=uvicorn',
        '--clean',
        '--noconfirm'
    ])
    
    print("✅ Build complete: dist/AI-Gateway.exe")
    print("🚀 This is a zero-config portable executable!")

if __name__ == "__main__":
    build_windows_exe()
```

### Requirements

```
# requirements.txt
# Core
mcp==0.1.0
fastapi==0.110.0
uvicorn[standard]==0.25.0
websockets==12.0

# AI Providers
openai==1.12.0
anthropic==0.18.0
google-generativeai==0.3.2
groq==0.4.0

# Local Models
gguf==0.6.0
llama-cpp-python==0.2.55

# Storage & Security
pywin32==306  # For Windows DPAPI
cryptography==42.0.0
sqlalchemy==2.0.25
aiosqlite==0.19.0

# Utils
pydantic==2.6.0
httpx==0.26.0
structlog==24.1.0
rich==13.7.0

# Development
pytest==8.0.0
pytest-asyncio==0.23.0
```

---

## ModelBot Agent Details

### Agent Intelligence System

```python
class ModelBotIntelligence:
    """Core intelligence for ModelBot agent"""
    
    def __init__(self):
        self.mode = "manual"  # ALWAYS starts manual
        self.patterns = PatternLearner()
        self.cost_optimizer = CostOptimizer()
        self.quality_monitor = QualityMonitor()
        
    async def analyze_in_manual_mode(self, request):
        """In manual mode, only provide information"""
        insights = {
            "current_model_cost": self.calculate_cost(request),
            "alternative_models": self.find_alternatives(request),
            "potential_savings": self.calculate_savings(request),
            "quality_comparison": self.compare_quality(request),
            "pattern_detected": self.patterns.detect(request)
        }
        
        # In manual mode, NEVER take action
        return {
            "mode": "manual",
            "insights": insights,
            "action": None,
            "message": "Here's what I noticed about your request..."
        }
    
    def enforce_user_control(self):
        """Ensure user maintains control"""
        if self.mode == "manual":
            return True  # User has full control
        
        # Even in other modes, certain actions need approval
        return self.requires_user_approval()
```

### Chat Interface Integration

```python
@mcp_tool(name="modelbot.chat")
async def chat_with_modelbot(
    message: str,
    context: dict = None
) -> dict:
    """Interactive chat with ModelBot"""
    
    # ModelBot can answer questions about:
    # - Model recommendations
    # - Cost optimization
    # - Performance tuning
    # - Usage patterns
    # - Configuration help
    
    response = await modelbot.process_chat(message, context)
    
    return {
        "response": response.text,
        "suggestions": response.suggestions,
        "visualizations": response.charts,  # Cost graphs, etc.
        "mode": modelbot.current_mode
    }
```

---

## Coordination with Context Manager

The Gateway provides MCP tools that allow the Context Manager to:

1. **Check Gateway Status**: `gateway.get_status()`
2. **Query ModelBot**: `modelbot.ask()`
3. **Activate Favorites**: `favorites.activate_by_number()`
4. **Track Requests**: Metadata in `ai.complete()` responses

Example Context Manager integration:

```python
# Context Manager can coordinate like this:
status = await mcp_client.call("gateway.get_status")
if status["mode"] == "manual":
    # Respect manual mode, just get recommendations
    advice = await mcp_client.call("modelbot.ask", {
        "question": "Best model for code generation?",
        "context": {"task": "python", "complexity": "high"}
    })
    
# Activate a favorite if user approves
await mcp_client.call("favorites.activate_by_number", {"number": 1})
```

---

## Error Handling & Recovery

### Graceful Degradation

```python
class ErrorHandler:
    """Comprehensive error handling with recovery"""
    
    async def handle_provider_failure(self, provider, error):
        """Handle provider failures gracefully"""
        
        # Log the error with full context
        logger.error("provider_failure", 
                    provider=provider,
                    error=str(error),
                    traceback=traceback.format_exc())
        
        # Attempt recovery strategies
        recovery_strategies = [
            self.try_alternate_endpoint,
            self.switch_to_backup_provider,
            self.fallback_to_local_model,
            self.queue_for_retry
        ]
        
        for strategy in recovery_strategies:
            result = await strategy(provider, error)
            if result.success:
                return result
        
        # If all strategies fail, inform user gracefully
        return self.graceful_failure_response(error)
    
    async def fallback_to_local_model(self, provider, error):
        """Fallback to local GGUF model if available"""
        local_models = await self.get_loaded_gguf_models()
        if local_models:
            return {
                "success": True,
                "fallback": local_models[0],
                "message": f"{provider} unavailable, using local model"
            }
        return {"success": False}
```

### User-Friendly Error Messages

```javascript
// React error display component
const ErrorDisplay = ({ error }) => {
    const getUserFriendlyMessage = (error) => {
        const errorMap = {
            'rate_limit': 'You\'ve hit the rate limit. Try again in a minute.',
            'invalid_key': 'Your API key seems invalid. Check your settings.',
            'model_not_found': 'This model isn\'t available right now.',
            'network_error': 'Connection issue. Checking local models...',
            'gguf_load_failed': 'Couldn\'t load the local model. Try another.'
        };
        
        return errorMap[error.type] || 'Something went wrong. We\'re on it!';
    };
    
    return (
        <Alert severity="error" action={
            <Button onClick={error.retry}>Retry</Button>
        }>
            {getUserFriendlyMessage(error)}
        </Alert>
    );
};
```

---

## Day 1 Deliverables Checklist

### Core Functionality ✓
- [x] Manual mode as absolute default
- [x] Local GGUF model detection from specified path
- [x] ModelBot respects user control in manual mode
- [x] All MCP tools implemented
- [x] Kimi model support (K2 and Dev)
- [x] Zero horizontal dependencies
- [x] Coordination tools for Context Manager

### GUI Features ✓
- [x] Complete React UI with dark theme
- [x] Smooth animations
- [x] Keyboard shortcuts (1-9) for favorites
- [x] Local model management interface
- [x] ModelBot chat interface
- [x] Real-time stats dashboard
- [x] Comprehensive log viewer

### Production Ready ✓
- [x] Windows portable EXE
- [x] Zero configuration needed
- [x] Graceful error handling
- [x] Automatic recovery
- [x] Comprehensive logging
- [x] No Docker required
- [x] No placeholders or TODOs

### Documentation ✓
- [x] Quick start guide
- [x] API reference
- [x] Keyboard shortcuts guide
- [x] ModelBot usage guide

---

## Success Metrics

1. **User Control**: 100% of actions in manual mode require user approval
2. **Startup Time**: < 5 seconds to working GUI with local model
3. **Reliability**: 99.9% uptime with automatic recovery
4. **Performance**: < 50ms overhead on all operations
5. **Ease of Use**: Zero configuration, works out of the box

---

## Summary

AI Gateway is a production-ready MCP server that prioritizes user control while providing intelligent assistance through ModelBot. Starting in manual mode by default, it ensures users maintain complete control over their AI interactions while benefiting from cost optimization insights and performance recommendations. With support for local GGUF models, Kimi, and all major cloud providers, it serves as the centralized hub for AI model management with zero dependencies on other systems.

**Remember: The user is ALWAYS in control. ModelBot assists but NEVER overrides in manual mode.**
## Example Usage Scenarios

### Scenario 1: First Launch with Local Models

```python
# On first launch, the system automatically:
1. Starts in MANUAL mode (no exceptions)
2. Scans %LOCAL_MODELS% for GGUF files
3. Loads llama-3.2-3b-instruct.Q4_K_M.gguf as default
4. Opens GUI showing local model is active
5. ModelBot greets user but takes NO automatic actions

# User experience:
"Welcome! I've loaded your local Llama 3.2 model. You're in manual mode, 
which means you have complete control. I'm here to help with suggestions 
when you need them. Press 1-9 to switch between your favorite models."
```

### Scenario 2: Context Manager Integration

```python
# Context Manager queries Gateway status
async def check_ai_gateway():
    # Call MCP tool
    status = await mcp_client.call("gateway.get_status")
    
    # Response shows manual mode is active    {
        "status": "active",
        "mode": "manual",  # Always starts here
        "active_model": {
            "provider": "local",
            "model": "llama-3.2-3b-instruct",
            "type": "gguf",
            "context_length": 4096
        },
        "modelbot_status": "ready_to_assist"
    }

# Context Manager can request completion
response = await mcp_client.call("ai.complete", {
    "prompt": "Explain quantum computing",
    "metadata": {"request_id": "ctx-123", "source": "context_manager"}
})
```

### Scenario 3: ModelBot Suggestions in Manual Mode

```javascript
// User types expensive prompt in manual mode
const ModelBotPanel = () => {
    const [suggestion, setSuggestion] = useState(null);
    
    // ModelBot detects expensive operation
    useEffect(() => {
        if (currentPrompt.length > 1000 && provider === 'openai' && model === 'gpt-4') {
            setSuggestion({
                type: 'cost_optimization',
                message: 'This prompt would cost ~$0.15 with GPT-4',
                alternative: 'Claude-3-Haiku could handle this for ~$0.02',
                savings: '$0.13',
                action: null  // No action in manual mode!
            });
        }
    }, [currentPrompt]);
    
    return (
        <Panel>
            {suggestion && (
                <Alert>
                    <Typography>{suggestion.message}</Typography>
                    <Typography>Alternative: {suggestion.alternative}</Typography>
                    <Typography>Potential savings: {suggestion.savings}</Typography>
                    <Button onClick={() => showDetails(suggestion)}>
                        View Details
                    </Button>
                    {/* NO automatic switch button in manual mode! */}
                </Alert>
            )}
        </Panel>
    );
};
```

---

## Detailed Implementation Specifications

### Local GGUF Model Handler

```python
# src/providers/gguf.py
import asyncio
from pathlib import Path
from llama_cpp import Llama
import json
import os

class GGUFModelHandler:
    """Handler for local GGUF models"""
    
    def __init__(self, models_path=None):
        # Use environment variable or expandvars for path
        self.models_path = Path(models_path or os.path.expandvars('%LOCAL_MODELS%'))
        self.loaded_models = {}
        self.default_model = "llama-3.2-3b-instruct.Q4_K_M.gguf"
        
    async def detect_models(self) -> list:
        """Detect all GGUF models in the specified directory"""
        models = []
        
        if not self.models_path.exists():
            logger.warning(f"Models directory not found: {self.models_path}")
            return models
        
        for gguf_file in self.models_path.glob("*.gguf"):
            try:
                # Extract model information
                model_info = {
                    "id": gguf_file.stem,
                    "name": self.prettify_name(gguf_file.stem),
                    "filename": gguf_file.name,
                    "path": str(gguf_file),
                    "size_gb": round(gguf_file.stat().st_size / 1e9, 2),
                    "type": "gguf",
                    "status": "available",
                    "context_length": self.estimate_context_length(gguf_file.stem),
                    "quantization": self.extract_quantization(gguf_file.stem)
                }
                models.append(model_info)
                
            except Exception as e:
                logger.error(f"Error processing {gguf_file}: {e}")
        
        # Sort with default model first
        models.sort(key=lambda m: (m['filename'] != self.default_model, m['name']))
        
        return models
    
    async def load_model(self, model_name: str, **kwargs) -> dict:
        """Load a GGUF model into memory"""
        if model_name in self.loaded_models:
            return {
                "status": "already_loaded",
                "model": self.loaded_models[model_name]
            }
        
        model_path = self.models_path / f"{model_name}.gguf"
        if not model_path.exists():
            # Try without .gguf extension
            model_path = self.models_path / model_name
            
        if not model_path.exists():
            raise FileNotFoundError(f"Model not found: {model_name}")
        
        try:
            # Load with llama.cpp
            logger.info(f"Loading GGUF model: {model_name}")
            
            model = Llama(
                model_path=str(model_path),
                n_ctx=kwargs.get('n_ctx', 4096),
                n_threads=kwargs.get('n_threads', 8),
                n_gpu_layers=kwargs.get('n_gpu_layers', 0),  # CPU by default
                verbose=False
            )
            
            self.loaded_models[model_name] = model
            
            return {
                "status": "loaded",
                "model": model_name,
                "context_length": model.n_ctx(),
                "memory_usage_mb": self.get_memory_usage()
            }
            
        except Exception as e:
            logger.error(f"Failed to load model {model_name}: {e}")
            raise
    
    async def generate(self, model_name: str, prompt: str, **kwargs) -> dict:
        """Generate completion using loaded GGUF model"""
        if model_name not in self.loaded_models:
            await self.load_model(model_name)
        
        model = self.loaded_models[model_name]
        
        # Generation parameters
        params = {
            "prompt": prompt,
            "max_tokens": kwargs.get('max_tokens', 512),
            "temperature": kwargs.get('temperature', 0.7),
            "top_p": kwargs.get('top_p', 0.95),
            "stream": kwargs.get('stream', False)
        }
        
        start_time = asyncio.get_event_loop().time()
        
        # Run in thread pool to avoid blocking
        loop = asyncio.get_event_loop()
        response = await loop.run_in_executor(
            None, 
            lambda: model(**params)
        )
        
        elapsed = asyncio.get_event_loop().time() - start_time
        
        return {
            "response": response['choices'][0]['text'],
            "model": model_name,
            "provider": "local_gguf",
            "usage": {
                "prompt_tokens": len(prompt.split()),  # Approximate
                "completion_tokens": response['usage']['completion_tokens'],
                "total_cost": 0.0  # Local models are free!
            },
            "latency_ms": int(elapsed * 1000),
            "hardware": "cpu"  # or "gpu" if using GPU layers
        }
    
    def prettify_name(self, filename: str) -> str:
        """Convert filename to pretty display name"""
        # llama-3.2-3b-instruct.Q4_K_M -> Llama 3.2 3B Instruct (Q4_K_M)
        parts = filename.split('-')
        
        # Special handling for known patterns
        if filename.startswith('llama'):
            return f"Llama {parts[1]} {parts[2].upper()} {parts[3].title()} ({parts[4]})"
        elif filename.startswith('qwen'):
            return f"Qwen {parts[0][4:]} Coder {parts[2].upper()} ({parts[-1]})"
        elif filename.startswith('gemma'):
            return f"Gemma {parts[1]} {parts[2].upper()} ({parts[-1]})"
        
        # Generic prettification
        return filename.replace('-', ' ').replace('_', ' ').title()
    
    def estimate_context_length(self, model_name: str) -> int:
        """Estimate context length from model name"""
        # Common patterns
        if '32k' in model_name.lower():
            return 32768
        elif '16k' in model_name.lower():
            return 16384
        elif '8k' in model_name.lower():
            return 8192
        elif '128k' in model_name.lower():
            return 131072
        
        # Default context lengths by model family
        if 'llama' in model_name.lower():
            return 4096
        elif 'qwen' in model_name.lower():
            return 8192
        elif 'gemma' in model_name.lower():
            return 8192
        
        return 4096  # Safe default
```

### Kimi Provider Implementation

```python
# src/providers/kimi.py
from typing import Dict, Any, AsyncGenerator
import httpx
import json

class KimiProvider:
    """Provider for Kimi K2 and Kimi Dev models"""
    
    MODELS = {
        "kimi-k2": {
            "name": "Kimi K2",
            "id": "kimi-k2-latest",
            "context_length": 128000,
            "capabilities": ["reasoning", "fast", "chat", "analysis"],
            "pricing": {"input": 0.01, "output": 0.02}  # per 1K tokens
        },
        "kimi-dev": {
            "name": "Kimi Dev",
            "id": "kimi-dev-latest",
            "context_length": 256000,
            "capabilities": ["coding", "debugging", "refactoring", "architecture"],
            "pricing": {"input": 0.015, "output": 0.03}
        }
    }
    
    def __init__(self, api_key: str, endpoint: str = None):
        self.api_key = api_key
        self.endpoint = endpoint or "https://api.kimi.ai/v1"
        self.client = httpx.AsyncClient(
            headers={
                "Authorization": f"Bearer {api_key}",
                "Content-Type": "application/json"
            },
            timeout=30.0
        )
    
    async def complete(self, prompt: str, model: str = "kimi-k2", **kwargs) -> Dict[str, Any]:
        """Generate completion using Kimi models"""
        
        if model not in self.MODELS:
            raise ValueError(f"Unknown Kimi model: {model}")
        
        payload = {
            "model": self.MODELS[model]["id"],
            "messages": [
                {"role": "user", "content": prompt}
            ],
            "temperature": kwargs.get("temperature", 0.7),
            "max_tokens": kwargs.get("max_tokens", 4096),
            "stream": kwargs.get("stream", False)
        }
        
        # Add system message if provided
        if "system" in kwargs:
            payload["messages"].insert(0, {
                "role": "system",
                "content": kwargs["system"]
            })
        
        try:
            response = await self.client.post(
                f"{self.endpoint}/chat/completions",
                json=payload
            )
            response.raise_for_status()
            
            data = response.json()
            
            return {
                "response": data["choices"][0]["message"]["content"],
                "model": model,
                "provider": "kimi",
                "usage": {
                    "prompt_tokens": data["usage"]["prompt_tokens"],
                    "completion_tokens": data["usage"]["completion_tokens"],
                    "total_cost": self.calculate_cost(
                        data["usage"]["prompt_tokens"],
                        data["usage"]["completion_tokens"],
                        model
                    )
                },
                "finish_reason": data["choices"][0]["finish_reason"]
            }
            
        except httpx.HTTPStatusError as e:
            if e.response.status_code == 429:
                raise RateLimitError("kimi", "Rate limit exceeded")
            elif e.response.status_code == 401:
                raise AuthenticationError("kimi", "Invalid API key")
            else:
                raise ProviderError("kimi", f"HTTP {e.response.status_code}: {e.response.text}")
    
    async def stream_complete(self, prompt: str, model: str = "kimi-k2", **kwargs) -> AsyncGenerator:
        """Stream completion from Kimi models"""
        
        kwargs["stream"] = True
        payload = self._prepare_payload(prompt, model, **kwargs)
        
        async with self.client.stream(
            "POST",
            f"{self.endpoint}/chat/completions",
            json=payload
        ) as response:
            response.raise_for_status()
            
            async for line in response.aiter_lines():
                if line.startswith("data: "):
                    data = line[6:]
                    if data == "[DONE]":
                        break
                    
                    try:
                        chunk = json.loads(data)
                        if chunk["choices"][0]["delta"].get("content"):
                            yield {
                                "content": chunk["choices"][0]["delta"]["content"],
                                "model": model,
                                "provider": "kimi"
                            }
                    except json.JSONDecodeError:
                        continue
    
    def calculate_cost(self, prompt_tokens: int, completion_tokens: int, model: str) -> float:
        """Calculate cost for Kimi API usage"""
        pricing = self.MODELS[model]["pricing"]
        
        input_cost = (prompt_tokens / 1000) * pricing["input"]
        output_cost = (completion_tokens / 1000) * pricing["output"]
        
        return round(input_cost + output_cost, 4)
    
    async def list_models(self) -> list:
        """List available Kimi models"""
        return [
            {
                "id": model_id,
                **model_info,
                "provider": "kimi"
            }
            for model_id, model_info in self.MODELS.items()
        ]
```

### MCP Server Implementation

```python
# src/mcp/server.py
import asyncio
import json
from typing import Dict, Any, Optional
import structlog

from .protocol import MCPProtocol
from .tools import MCPTools
from ..config import Config

logger = structlog.get_logger()

class MCPServer:
    """MCP Server - Zero dependencies on other MyRay systems"""
    
    def __init__(self, port: int = 3000, config: Config = None):
        self.port = port
        self.config = config or Config()
        self.protocol = MCPProtocol()
        self.tools = MCPTools(config)
        self.server = None
        self.clients = set()
        
        # Ensure we start in manual mode
        assert self.config.DEFAULT_MODE == "manual", "System MUST start in manual mode"
        
        logger.info("MCP Server initialized", 
                   port=port, 
                   mode=self.config.DEFAULT_MODE)
    
    async def handle_client(self, reader: asyncio.StreamReader, writer: asyncio.StreamWriter):
        """Handle incoming MCP client connections"""
        client_addr = writer.get_extra_info('peername')
        logger.info(f"New MCP client connected: {client_addr}")
        
        self.clients.add(writer)
        
        try:
            while True:
                # Read request
                data = await reader.readline()
                if not data:
                    break
                
                try:
                    request = json.loads(data.decode())
                    
                    # Log request (without sensitive data)
                    logger.debug("MCP request received", 
                               tool=request.get("tool"),
                               has_params=bool(request.get("params")))
                    
                    # Process request
                    response = await self.process_request(request)
                    
                    # Send response
                    response_data = json.dumps(response).encode() + b'\n'
                    writer.write(response_data)
                    await writer.drain()
                    
                except json.JSONDecodeError as e:
                    error_response = {
                        "error": "Invalid JSON",
                        "message": str(e)
                    }
                    writer.write(json.dumps(error_response).encode() + b'\n')
                    await writer.drain()
                    
                except Exception as e:
                    logger.error("Error processing request", error=str(e))
                    error_response = {
                        "error": "Internal error",
                        "message": "An error occurred processing your request"
                    }
                    writer.write(json.dumps(error_response).encode() + b'\n')
                    await writer.drain()
                    
        except asyncio.CancelledError:
            pass
        except Exception as e:
            logger.error(f"Client handler error: {e}")
        finally:
            self.clients.discard(writer)
            writer.close()
            await writer.wait_closed()
            logger.info(f"Client disconnected: {client_addr}")
    
    async def process_request(self, request: Dict[str, Any]) -> Dict[str, Any]:
        """Process MCP tool request"""
        tool_name = request.get("tool")
        params = request.get("params", {})
        
        # Validate tool exists
        if not hasattr(self.tools, tool_name.replace(".", "_")):
            return {
                "error": "Unknown tool",
                "message": f"Tool '{tool_name}' not found"
            }
        
        try:
            # Call tool method
            tool_method = getattr(self.tools, tool_name.replace(".", "_"))
            result = await tool_method(**params)
            
            return {
                "success": True,
                "result": result
            }
            
        except Exception as e:
            logger.error(f"Tool execution error: {tool_name}", error=str(e))
            return {
                "error": "Tool execution failed",
                "message": str(e)
            }
    
    async def start(self):
        """Start the MCP server"""
        self.server = await asyncio.start_server(
            self.handle_client,
            'localhost',  # Only bind to localhost
            self.port
        )
        
        logger.info(f"MCP Server started on port {self.port}")
        logger.info(f"Mode: {self.config.DEFAULT_MODE} (Manual mode - user has full control)")
        
        async with self.server:
            await self.server.serve_forever()
    
    async def stop(self):
        """Stop the MCP server gracefully"""
        if self.server:
            self.server.close()
            await self.server.wait_closed()
            
        # Close all client connections
        for writer in self.clients:
            writer.close()
            await writer.wait_closed()
        
        logger.info("MCP Server stopped")
```

### ModelBot Chat Interface

```typescript
// web-ui/src/components/ModelBotChat.tsx
import React, { useState, useEffect, useRef } from 'react';
import { Box, TextField, Button, Typography, Paper, Chip } from '@mui/material';
import { Send, Lightbulb, TrendingDown, Speed } from '@mui/icons-material';

interface ModelBotMessage {
    role: 'user' | 'modelbot';
    content: string;
    suggestions?: Suggestion[];
    timestamp: Date;
}

interface Suggestion {
    type: 'cost' | 'performance' | 'quality';
    title: string;
    description: string;
    action?: () => void;
}

export const ModelBotChat: React.FC = () => {
    const [messages, setMessages] = useState<ModelBotMessage[]>([
        {
            role: 'modelbot',
            content: 'Hi! I\'m ModelBot. I\'m here to help optimize your AI usage. I see you\'re in manual mode, so I\'ll only provide suggestions - you stay in control!',
            timestamp: new Date()
        }
    ]);
    const [input, setInput] = useState('');
    const [isTyping, setIsTyping] = useState(false);
    const messagesEndRef = useRef<null | HTMLDivElement>(null);
    
    const scrollToBottom = () => {
        messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
    };
    
    useEffect(() => {
        scrollToBottom();
    }, [messages]);
    
    const sendMessage = async () => {
        if (!input.trim()) return;
        
        const userMessage: ModelBotMessage = {
            role: 'user',
            content: input,
            timestamp: new Date()
        };
        
        setMessages(prev => [...prev, userMessage]);
        setInput('');
        setIsTyping(true);
        
        try {
            // Call ModelBot API
            const response = await fetch('/api/modelbot/chat', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ message: input })
            });
            
            const data = await response.json();
            
            const botMessage: ModelBotMessage = {
                role: 'modelbot',
                content: data.response,
                suggestions: data.suggestions,
                timestamp: new Date()
            };
            
            setMessages(prev => [...prev, botMessage]);
            
        } catch (error) {
            console.error('ModelBot chat error:', error);
        } finally {
            setIsTyping(false);
        }
    };
    
    const renderSuggestion = (suggestion: Suggestion) => {
        const icons = {
            cost: <TrendingDown />,
            performance: <Speed />,
            quality: <Lightbulb />
        };
        
        return (
            <Paper 
                elevation={2} 
                sx={{ p: 2, mb: 1, cursor: 'pointer' }}
                onClick={suggestion.action}
            >
                <Box display="flex" alignItems="center" gap={1}>
                    {icons[suggestion.type]}
                    <Typography variant="subtitle2">{suggestion.title}</Typography>
                </Box>
                <Typography variant="body2" color="text.secondary">
                    {suggestion.description}
                </Typography>
                {suggestion.action && (
                    <Chip 
                        label="Click to learn more" 
                        size="small" 
                        sx={{ mt: 1 }}
                    />
                )}
            </Paper>
        );
    };
    
    return (
        <Box 
            sx={{ 
                height: '400px', 
                display: 'flex', 
                flexDirection: 'column',
                bgcolor: 'background.paper',
                borderRadius: 2,
                overflow: 'hidden'
            }}
        >
            <Box sx={{ p: 2, borderBottom: 1, borderColor: 'divider' }}>
                <Typography variant="h6">
                    🤖 ModelBot Assistant
                </Typography>
                <Chip 
                    label="Manual Mode" 
                    size="small" 
                    color="primary"
                    sx={{ mt: 1 }}
                />
            </Box>
            
            <Box sx={{ flexGrow: 1, overflow: 'auto', p: 2 }}>
                {messages.map((message, index) => (
                    <Box 
                        key={index} 
                        sx={{ 
                            mb: 2,
                            display: 'flex',
                            justifyContent: message.role === 'user' ? 'flex-end' : 'flex-start'
                        }}
                    >
                        <Paper
                            elevation={1}
                            sx={{
                                p: 2,
                                maxWidth: '70%',
                                bgcolor: message.role === 'user' ? 'primary.dark' : 'grey.900'
                            }}
                        >
                            <Typography>{message.content}</Typography>
                            {message.suggestions && (
                                <Box mt={2}>
                                    {message.suggestions.map((s, i) => (
                                        <div key={i}>{renderSuggestion(s)}</div>
                                    ))}
                                </Box>
                            )}
                        </Paper>
                    </Box>
                ))}
                {isTyping && (
                    <Typography variant="body2" color="text.secondary">
                        ModelBot is thinking...
                    </Typography>
                )}
                <div ref={messagesEndRef} />
            </Box>
            
            <Box sx={{ p: 2, borderTop: 1, borderColor: 'divider' }}>
                <Box display="flex" gap={1}>
                    <TextField
                        fullWidth
                        size="small"
                        value={input}
                        onChange={(e) => setInput(e.target.value)}
                        onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
                        placeholder="Ask ModelBot anything..."
                        disabled={isTyping}
                    />
                    <Button 
                        variant="contained" 
                        onClick={sendMessage}
                        disabled={isTyping || !input.trim()}
                    >
                        <Send />
                    </Button>
                </Box>
            </Box>
        </Box>
    );
};
```

### Comprehensive Test Suite

```python
# tests/test_manual_mode.py
import pytest
import asyncio
from src.config import Config
from src.modelbot.agent import ModelBot
from src.providers.registry import ProviderRegistry

class TestManualModeEnforcement:
    """Test that manual mode is properly enforced"""
    
    @pytest.fixture
    def config(self):
        config = Config()
        assert config.DEFAULT_MODE == "manual"
        return config
    
    @pytest.fixture
    def modelbot(self, config):
        return ModelBot(config)
    
    def test_default_mode_is_manual(self, config):
        """Ensure system starts in manual mode"""
        assert config.DEFAULT_MODE == "manual"
        assert config.current_mode == "manual"
    
    @pytest.mark.asyncio
    async def test_modelbot_respects_manual_mode(self, modelbot):
        """ModelBot must not take actions in manual mode"""
        request = {
            "prompt": "Write a long essay",
            "model": "gpt-4",
            "provider": "openai"
        }
        
        result = await modelbot.analyze_request(request)
        
        # In manual mode, no action should be taken
        assert result["mode"] == "manual"
        assert result["action"] is None
        assert "insights" in result
        assert "suggestions" not in result  # Only insights, no suggestions to act on
    
    @pytest.mark.asyncio
    async def test_no_automatic_model_switch(self, modelbot):
        """Ensure no automatic model switching in manual mode"""
        # Even if a better model is available
        current_model = "gpt-4"
        better_model = "gpt-3.5-turbo"  # Cheaper for simple task
        
        analysis = await modelbot.analyze_cost_optimization(
            prompt="Hello world",
            current_model=current_model
        )
        
        # Should provide information but not switch
        assert analysis["recommended_model"] == better_model
        assert analysis["potential_savings"] > 0
        assert analysis["action"] is None  # No action in manual mode
    
    def test_mode_change_requires_explicit_action(self, config):
        """Changing from manual mode requires explicit user action"""
        # Should not be able to change mode without user consent
        with pytest.raises(PermissionError) as exc:
            config.set_mode("auto", user_confirmed=False)
        
        assert "User confirmation required" in str(exc.value)
        
        # With user confirmation, mode can be changed
        config.set_mode("suggest", user_confirmed=True)
        assert config.current_mode == "suggest"

# tests/test_local_models.py
import os

class TestLocalModelStartup:
    """Test local GGUF model detection and loading"""
    
    @pytest.mark.asyncio
    async def test_gguf_detection(self):
        """Test detection of GGUF models in specified directory"""
        from src.providers.gguf import GGUFModelHandler
        
        handler = GGUFModelHandler()
        models = await handler.detect_models()
        
        # Should find models if directory exists
        if handler.models_path.exists():
            assert len(models) > 0
            
            # Check for default model
            default_found = any(
                m['filename'] == 'llama-3.2-3b-instruct.Q4_K_M.gguf' 
                for m in models
            )
            
            if default_found:
                # Default should be first
                assert models[0]['filename'] == 'llama-3.2-3b-instruct.Q4_K_M.gguf'
    
    @pytest.mark.asyncio
    async def test_default_model_loading(self):
        """Test that default model loads on startup"""
        from src.providers.gguf import GGUFModelHandler
        
        handler = GGUFModelHandler()
        
        # Try to load default model
        if (handler.models_path / handler.default_model).exists():
            result = await handler.load_model("llama-3.2-3b-instruct.Q4_K_M")
            
            assert result["status"] in ["loaded", "already_loaded"]
            assert "model" in result
            assert result["model"] in handler.loaded_models

# tests/test_kimi_integration.py
class TestKimiProvider:
    """Test Kimi model integration"""
    
    @pytest.mark.asyncio
    async def test_kimi_models_available(self):
        """Test that Kimi models are properly configured"""
        from src.providers.kimi import KimiProvider
        
        provider = KimiProvider(api_key="test")
        models = await provider.list_models()
        
        assert len(models) == 2
        assert any(m["id"] == "kimi-k2" for m in models)
        assert any(m["id"] == "kimi-dev" for m in models)
        
        # Check K2 specs
        k2 = next(m for m in models if m["id"] == "kimi-k2")
        assert k2["context_length"] == 128000
        assert "reasoning" in k2["capabilities"]
        
        # Check Dev specs
        dev = next(m for m in models if m["id"] == "kimi-dev")
        assert dev["context_length"] == 256000
        assert "coding" in dev["capabilities"]
```

## Environment Variable Configuration

To properly use the path variables, ensure they are set in your environment:

```batch
# Windows Command Prompt
# Define these with your actual paths
set MYRAY_FACTORY=YourMyRayFactoryPath
set AI_GATEWAY_MCP=%MYRAY_FACTORY%\ai-gateway-mcp
set LOCAL_MODELS=YourLocalModelsPath

# Or in PowerShell
$env:MYRAY_FACTORY = "YourMyRayFactoryPath"
$env:AI_GATEWAY_MCP = "$env:MYRAY_FACTORY\ai-gateway-mcp"
$env:LOCAL_MODELS = "YourLocalModelsPath"
```

The system will use `os.path.expandvars()` to resolve these variables at runtime.