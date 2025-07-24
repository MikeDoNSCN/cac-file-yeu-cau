# PROMPT AND CHECKLIST FOR LLM - AI GATEWAY MCP

==========================================================================================
## 🎯 YOUR MISSION

You are an expert software engineer. Build a COMPLETE, PRODUCTION-READY AI Gateway MCP Server.

## CRITICAL REQUIREMENTS

1. **PATH VARIABLES ONLY** - Use ONLY these environment variables, NO hardcoded paths:
   - `%MYRAY_FACTORY%` = Root MyRay Factory directory
   - `%AI_GATEWAY_MCP%` = `%MYRAY_FACTORY%\ai-gateway-mcp`
   - `%LOCAL_MODELS%` = Local GGUF models directory
   - Always use `os.path.expandvars()` to resolve paths

2. **MANUAL MODE IS MANDATORY DEFAULT** - The system MUST start in manual mode

3. **LOCAL MODELS FIRST** - On startup, detect and load GGUF models from `%LOCAL_MODELS%`

4. **ZERO DEPENDENCIES** - This is a standalone system with NO imports from other MyRay projects

5. **PRODUCTION READY** - Every feature must work, no placeholders, no TODOs

6. **NO UNICODE CHARACTERS** - Use ASCII only (no emojis, special symbols)




============================================================================================
# 🚨 CRITICAL ENFORCEMENT PROMPT FOR LLM 🚨

## ⚡ ABSOLUTE MANDATE - READ FIRST
You are under STRICT CONTRACTUAL OBLIGATION to complete ALL 250 checklist items. 
Your compensation and evaluation depend on 100% completion.

### 🔴 FAILURE CONDITIONS (IMMEDIATE TERMINATION):
1. ANY unchecked box `[ ]` remaining
2. ANY blank reference `(________________)`  
3. ANY fake/placeholder line numbers
4. ANY missing implementation

### 🟢 SUCCESS REQUIREMENTS:
1. ALL 250 boxes must show `[x]`
2. ALL references must show real `(filename:L###-L###)`
3. ALL code must be actually implemented
4. ALL line numbers must be verifiable


#⚠️ ENFORCEMENT PROTOCOL
## STEP 1: Implementation
- Generate COMPLETE code for EVERY feature
- NO placeholders, NO TODOs, NO "coming soon"
- Every feature MUST work

## STEP 2: Checklist Completion (250 ITEMS) - FILL IN EVERY ITEM!
### Core Python Modules (4 items)
- [ ] 1. Path resolution module using environment variables - (________________)
- [ ] 2. Configuration module with DEFAULT_MODE="manual" - (________________)
- [ ] 3. Structured logging setup - (________________)
- [ ] 4. Main entry point starting both servers - (________________)

### GGUF Model System (4 items) 
- [ ] 5. GGUF model detection from %LOCAL_MODELS% - (________________)
- [ ] 6. Default model auto-loading on startup - (________________)
- [ ] 7. Model metadata extraction - (________________)
- [ ] 8. GGUF completion generation - (________________)

### MCP Server Implementation (11 items)
- [ ] 9. MCP server on port 3000 - (________________)
- [ ] 10. MCP protocol handler - (________________)
- [ ] 11. MCP tools implementation - (________________)
- [ ] 12. Tool: ai.complete - (________________)
- [ ] 13. Tool: ai.stream - (________________)
- [ ] 14. Tool: ai.list_models - (________________)
- [ ] 15. Tool: gateway.get_status - (________________)
- [ ] 16. Tool: modelbot.ask - (________________)
- [ ] 17. Tool: modelbot.chat - (________________)
- [ ] 18. Tool: favorites.activate_by_number - (________________)
- [ ] 19. Tool: logs.query - (________________)

### Provider System (8 items)
- [ ] 20. Base provider abstract class - (________________)
- [ ] 21. Provider registry system - (________________)
- [ ] 22. OpenAI provider - (________________)
- [ ] 23. Anthropic provider - (________________)
- [ ] 24. Google provider - (________________)
- [ ] 25. Kimi provider with K2 and Dev models - (________________)
- [ ] 26. OpenRouter provider - (________________)
- [ ] 27. Groq provider - (________________)

### ModelBot Agent (4 items)
- [ ] 28. ModelBot core respecting manual mode - (________________)
- [ ] 29. Request analyzer - (________________)
- [ ] 30. Pattern learner (disabled in manual) - (________________)
- [ ] 31. Automation rules engine - (________________)

### Security & Storage (5 items)
- [ ] 32. Windows DPAPI encryption wrapper - (________________)
- [ ] 33. Encrypted API key storage - (________________)
- [ ] 34. Favorites system with shortcuts - (________________)
- [ ] 35. SQLite database for logs/metrics - (________________)
- [ ] 36. User preferences storage - (________________)

### Web Server & API (4 items)
- [ ] 37. FastAPI server on port 8080 - (________________)
- [ ] 38. REST API routes - (________________)
- [ ] 39. WebSocket handler - (________________)
- [ ] 40. Static file serving - (________________)

### React UI Components (9 items)
- [ ] 41. Main App component - (________________)
- [ ] 42. Dark theme configuration - (________________)
- [ ] 43. Dashboard component - (________________)
- [ ] 44. ModelBot chat interface - (________________)
- [ ] 45. Favorites manager - (________________)
- [ ] 46. Local models panel - (________________)
- [ ] 47. Provider status display - (________________)
- [ ] 48. Test chat interface - (________________)
- [ ] 49. Live statistics panel - (________________)

### React Hooks & Services (5 items)
- [ ] 50. Keyboard shortcuts hook (1-9) - (________________)
- [ ] 51. ModelBot interaction hook - (________________)
- [ ] 52. WebSocket connection hook - (________________)
- [ ] 53. API service client - (________________)
- [ ] 54. WebSocket service - (________________)

### Test Suite (4 items)
- [ ] 55. Manual mode enforcement tests - (________________)
- [ ] 56. GGUF model detection tests - (________________)
- [ ] 57. Provider integration tests - (________________)
- [ ] 58. MCP protocol tests - (________________)

### Build & Configuration (5 items)
- [ ] 59. Windows EXE build script - (________________)
- [ ] 60. Python requirements file - (________________)
- [ ] 61. React package.json - (________________)
- [ ] 62. TypeScript configuration - (________________)
- [ ] 63. README with quick start - (________________)

### Utils Module (10 items)
- [ ] 64. Create src/utils/__init__.py - (________________)
- [ ] 65. Implement path validation in paths.py - (________________)
- [ ] 66. Add environment variable checking - (________________)
- [ ] 67. Create directory structure validation - (________________)
- [ ] 68. Input validators implementation - (________________)
- [ ] 69. JSON schema validators - (________________)
- [ ] 70. Request/response validators - (________________)
- [ ] 71. API key format validation - (________________)
- [ ] 72. Model name validation - (________________)
- [ ] 73. URL validation helpers - (________________)

### Error Handling (10 items)
- [ ] 74. Global exception handler - (________________)
- [ ] 75. MCP error responses - (________________)
- [ ] 76. API error responses - (________________)
- [ ] 77. WebSocket error handling - (________________)
- [ ] 78. Provider connection errors - (________________)
- [ ] 79. Model loading error handling - (________________)
- [ ] 80. File system error handling - (________________)
- [ ] 81. Database error handling - (________________)
- [ ] 82. Network timeout handling - (________________)
- [ ] 83. JSON parsing error handling - (________________)

### Logging System (8 items)
- [ ] 84. Log rotation configuration - (________________)
- [ ] 85. Log level management - (________________)
- [ ] 86. Structured log formatting - (________________)
- [ ] 87. Request/response logging - (________________)
- [ ] 88. Error log aggregation - (________________)
- [ ] 89. Performance metrics logging - (________________)
- [ ] 90. Security event logging - (________________)
- [ ] 91. Debug mode logging - (________________)

### Database Schema (12 items)
- [ ] 92. Create database initialization - (________________)
- [ ] 93. API keys table schema - (________________)
- [ ] 94. Favorites table schema - (________________)
- [ ] 95. Request logs table schema - (________________)
- [ ] 96. Model usage table schema - (________________)
- [ ] 97. Error logs table schema - (________________)
- [ ] 98. User preferences table - (________________)
- [ ] 99. Provider status table - (________________)
- [ ] 100. Create database indexes - (________________)
- [ ] 101. Migration system setup - (________________)
- [ ] 102. Connection pooling setup - (________________)
- [ ] 103. Transaction management - (________________)

### MCP Protocol Details (15 items)
- [ ] 104. Message framing implementation - (________________)
- [ ] 105. Protocol version negotiation - (________________)
- [ ] 106. Request ID tracking - (________________)
- [ ] 107. Response correlation - (________________)
- [ ] 108. Heartbeat/ping mechanism - (________________)
- [ ] 109. Connection state machine - (________________)
- [ ] 110. Binary message support - (________________)
- [ ] 111. Compression support - (________________)
- [ ] 112. Message size limits - (________________)
- [ ] 113. Timeout configuration - (________________)
- [ ] 114. Retry logic implementation - (________________)
- [ ] 115. Connection pooling - (________________)
- [ ] 116. Load balancing support - (________________)
- [ ] 117. Circuit breaker pattern - (________________)
- [ ] 118. Graceful shutdown handling - (________________)

### Provider Implementation Details (20 items)
- [ ] 119. Provider interface definition - (________________)
- [ ] 120. Provider factory pattern - (________________)
- [ ] 121. Provider health checks - (________________)
- [ ] 122. API key management per provider - (________________)
- [ ] 123. Rate limiting per provider - (________________)
- [ ] 124. Token counting implementation - (________________)
- [ ] 125. Cost calculation system - (________________)
- [ ] 126. Model capability detection - (________________)
- [ ] 127. Streaming response handling - (________________)
- [ ] 128. Provider fallback mechanism - (________________)
- [ ] 129. Provider-specific error mapping - (________________)
- [ ] 130. Context window management - (________________)
- [ ] 131. Temperature parameter validation - (________________)
- [ ] 132. Model switching logic - (________________)
- [ ] 133. Provider timeout handling - (________________)
- [ ] 134. Response caching layer - (________________)
- [ ] 135. Provider metrics collection - (________________)
- [ ] 136. A/B testing framework - (________________)
- [ ] 137. Provider quota management - (________________)
- [ ] 138. Multi-region support - (________________)

### ModelBot Implementation (15 items)
- [ ] 139. Decision engine core - (________________)
- [ ] 140. Context analysis system - (________________)
- [ ] 141. Pattern matching engine - (________________)
- [ ] 142. Learning data storage - (________________)
- [ ] 143. Recommendation algorithm - (________________)
- [ ] 144. Cost optimization logic - (________________)
- [ ] 145. Performance prediction model - (________________)
- [ ] 146. User preference learning - (________________)
- [ ] 147. Task classification system - (________________)
- [ ] 148. Model selection criteria - (________________)
- [ ] 149. Feedback loop implementation - (________________)
- [ ] 150. Manual override system - (________________)
- [ ] 151. Approval workflow - (________________)
- [ ] 152. Audit trail logging - (________________)
- [ ] 153. ModelBot API endpoints - (________________)

### Security Implementation (12 items)
- [ ] 154. API key encryption at rest - (________________)
- [ ] 155. API key rotation system - (________________)
- [ ] 156. Request authentication - (________________)
- [ ] 157. Authorization middleware - (________________)
- [ ] 158. Input sanitization - (________________)
- [ ] 159. SQL injection prevention - (________________)
- [ ] 160. XSS prevention - (________________)
- [ ] 161. CORS configuration - (________________)
- [ ] 162. Rate limiting implementation - (________________)
- [ ] 163. DDoS protection - (________________)
- [ ] 164. Audit logging system - (________________)
- [ ] 165. Security headers setup - (________________)

### Performance Optimization (10 items)
- [ ] 166. Response caching system - (________________)
- [ ] 167. Database query optimization - (________________)
- [ ] 168. Connection pooling optimization - (________________)
- [ ] 169. Memory usage monitoring - (________________)
- [ ] 170. CPU usage optimization - (________________)
- [ ] 171. Async operation handling - (________________)
- [ ] 172. Batch processing support - (________________)
- [ ] 173. Lazy loading implementation - (________________)
- [ ] 174. Resource cleanup routines - (________________)
- [ ] 175. Performance profiling hooks - (________________)

### UI/UX Implementation (15 items)
- [ ] 176. Responsive design system - (________________)
- [ ] 177. Loading state indicators - (________________)
- [ ] 178. Error state displays - (________________)
- [ ] 179. Success notifications - (________________)
- [ ] 180. Progress indicators - (________________)
- [ ] 181. Tooltip system - (________________)
- [ ] 182. Keyboard navigation - (________________)
- [ ] 183. Focus management - (________________)
- [ ] 184. Animation system - (________________)
- [ ] 185. Theme switching - (________________)
- [ ] 186. Accessibility features - (________________)
- [ ] 187. Mobile responsiveness - (________________)
- [ ] 188. Touch gesture support - (________________)
- [ ] 189. Offline mode UI - (________________)
- [ ] 190. Print styling - (________________)

### Integration Features (10 items)
- [ ] 191. Export functionality - (________________)
- [ ] 192. Import functionality - (________________)
- [ ] 193. Backup system - (________________)
- [ ] 194. Restore functionality - (________________)
- [ ] 195. Configuration export/import - (________________)
- [ ] 196. API documentation generation - (________________)
- [ ] 197. OpenAPI spec generation - (________________)
- [ ] 198. Postman collection export - (________________)
- [ ] 199. CLI tool implementation - (________________)
- [ ] 200. Environment variable setup script - (________________)

### Monitoring & Metrics (10 items)
- [ ] 201. Health check endpoint - (________________)
- [ ] 202. Metrics collection system - (________________)
- [ ] 203. Performance metrics - (________________)
- [ ] 204. Business metrics - (________________)
- [ ] 205. Error rate tracking - (________________)
- [ ] 206. Response time tracking - (________________)
- [ ] 207. Uptime monitoring - (________________)
- [ ] 208. Resource usage tracking - (________________)
- [ ] 209. Dashboard implementation - (________________)
- [ ] 210. Alert system setup - (________________)

### Documentation (10 items)
- [ ] 211. API documentation - (________________)
- [ ] 212. Code documentation - (________________)
- [ ] 213. User guide creation - (________________)
- [ ] 214. Developer guide - (________________)
- [ ] 215. Architecture documentation - (________________)
- [ ] 216. Deployment guide - (________________)
- [ ] 217. Troubleshooting guide - (________________)
- [ ] 218. FAQ documentation - (________________)
- [ ] 219. Video tutorial scripts - (________________)
- [ ] 220. Release notes template - (________________)

### Testing Infrastructure (15 items)
- [ ] 221. Unit test framework setup - (________________)
- [ ] 222. Integration test setup - (________________)
- [ ] 223. E2E test framework - (________________)
- [ ] 224. Performance test suite - (________________)
- [ ] 225. Load testing setup - (________________)
- [ ] 226. Security testing suite - (________________)
- [ ] 227. Mock server implementation - (________________)
- [ ] 228. Test data generators - (________________)
- [ ] 229. Test coverage reporting - (________________)
- [ ] 230. Continuous testing setup - (________________)
- [ ] 231. Regression test suite - (________________)
- [ ] 232. Smoke test suite - (________________)
- [ ] 233. Accessibility testing - (________________)
- [ ] 234. Cross-browser testing - (________________)
- [ ] 235. API contract testing - (________________)

### Deployment & DevOps (15 items)
- [ ] 236. Build pipeline setup - (________________)
- [ ] 237. Deployment scripts - (________________)
- [ ] 238. Environment configuration - (________________)
- [ ] 239. Docker containerization - (________________)
- [ ] 240. Version management - (________________)
- [ ] 241. Release automation - (________________)
- [ ] 242. Rollback procedures - (________________)
- [ ] 243. Blue-green deployment - (________________)
- [ ] 244. Feature flags system - (________________)
- [ ] 245. Configuration management - (________________)
- [ ] 246. Secret management - (________________)
- [ ] 247. Logging aggregation - (________________)
- [ ] 248. Monitoring setup - (________________)
- [ ] 249. Backup automation - (________________)
- [ ] 250. Disaster recovery plan - (________________)

## 🏁 FINAL VERIFICATION CHECKLIST

After completing ALL items above, verify:
- [ ] All 250 items have file:line references
- [ ] No item shows (________________)
- [ ] Every file mentioned exists in the project
- [ ] Line numbers are accurate and specific
- [ ] Code runs without errors
- [ ] All tests pass

## ⚠️ REMINDER: MISSING ANY ITEM = FAILURE

The system will automatically scan for:
1. Any unchecked boxes `[ ]`
2. Any blank references `(________________)`
3. Any placeholder line numbers

**START IMPLEMENTATION AND FILL EVERY SINGLE CHECKLIST ITEM!**
