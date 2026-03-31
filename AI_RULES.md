# AI Development Rules - FreeAPI Pool

This document defines the technical stack and development standards for the FreeAPI Pool project.

## Tech Stack
*   **Backend Framework**: FastAPI (Python 3.11+) for high-performance asynchronous API development.
*   **Web Server**: Uvicorn as the ASGI server implementation.
*   **Asynchronous I/O**: `aiohttp` for all external HTTP requests to AI providers.
*   **Data Validation**: Pydantic v2 for request/response schema modeling and environment variable management.
*   **Frontend**: Vanilla JavaScript (ES6+), HTML5, and CSS3. No build tools (Vite/Webpack) are used to keep the UI lightweight.
*   **API Standard**: Strict adherence to the OpenAI API specification (`/v1/chat/completions`, `/v1/models`).
*   **Containerization**: Docker and Docker Compose for orchestration.

## Library & Implementation Rules

### 1. Backend Development
*   **Routing**: All API endpoints must be defined in `api/routes.py` or separate modules within the `api/` directory.
*   **External Requests**: Never use synchronous libraries like `requests`. Always use `aiohttp` within an `async with` block to prevent blocking the event loop.
*   **Schema**: Use Pydantic models for all API request bodies and responses to ensure automatic documentation and validation.
*   **AI Providers**: New providers must inherit from `BaseProvider` in `providers/base.py` and implement the `create_completion` async generator.
*   **Model Registry**: All supported models must be registered in `models.py` using the `Model` dataclass.

### 2. Frontend Development
*   **Simplicity**: Keep the frontend as a Single Page Application (SPA) using vanilla JS. Avoid adding frameworks like React, Vue, or Tailwind unless a project-wide refactor is explicitly requested.
*   **State Management**: Use simple JavaScript objects and DOM manipulation for state.
*   **Styling**: Maintain the dark theme using CSS variables defined in `gui/static/css/style.css`.
*   **Icons**: Use inline SVGs or Lucide-style SVG paths.

### 3. Reliability & Performance
*   **Error Handling**: Implement graceful failure in providers. If a provider fails, it should throw an exception that the `IterListProvider` can catch to move to the next candidate.
*   **Streaming**: Always prioritize SSE (Server-Sent Events) for chat completions to provide a better user experience.
*   **Logging**: Use Python's standard `logging` module or simple print statements for critical server events.

## Project Structure
*   `api/`: API logic and authentication middleware.
*   `gui/`: Web interface (templates and static assets).
*   `providers/`: Individual AI provider implementations.
*   `models.py`: Central registry for models and provider mapping.
*   `main.py`: Application entry point.