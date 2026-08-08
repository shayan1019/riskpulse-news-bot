# System flow

1. An adapter receives a headline from a configured provider.
2. A typed event normalizes public fields such as title, category, and severity.
3. The service deduplicates and classifies the event.
4. Subscriber preferences select delivery language and alert relevance.
5. A Telegram delivery client formats the safe localized message.

The included example is deterministic and does not contact an external service.
