# Architecture

The original project is organized around adapters, models, application entry points, delivery, persistence, and services. News providers pass through an asynchronous adapter boundary, are normalized into typed events, then filtered and localized according to subscriber preferences before Telegram delivery. SQLite provides local application persistence.

The public artifact contains documentation and synthetic fixtures only until every selected production source file has passed a separate security review.
