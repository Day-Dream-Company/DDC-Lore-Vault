```mermaid
flowchart TD
    A[Start] --> B{Decision?}
    B -- Yes --> C[Do Thing]
    B -- No --> D[Do Other Thing]
    C --> E[End]
    D --> E
