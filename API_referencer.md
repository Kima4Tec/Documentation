```mermaid
flowchart TD
    A[API Lag - FilmFrame.Api] --> B[Application Lag - FilmFrame.Application]
    B --> C[Domain Lag - FilmFrame.Domain]
    B --> D[Infrastructure Lag - FilmFrame.Infrastructure]
    D --> C
    E[Unit Tests - FilmFrame.Tests] --> B
    E --> C

