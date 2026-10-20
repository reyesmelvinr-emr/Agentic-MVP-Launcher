# Least-Privilege Integration Identities

Live integrations use identities scoped to the minimum required access, separating read and write identities where the platform supports meaningful separation and preferring protected pipelines for writes. Each organization must approve the actual identity topology before live connections; if the active identity is broader than the approved action and cannot be constrained, that action is unavailable.
